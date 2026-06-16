
# Server Side Data Tables

To handle server-side pagination with pymssql (raw SQL Server connectivity), you must switch your pagination syntax from MySQL/PostgreSQL LIMIT/OFFSET to Microsoft SQL Server’s native OFFSET X ROWS FETCH NEXT Y ROWS ONLY clause.
Two critical nuances must be addressed when building raw queries for Bootstrap Table:

   1. Dynamic column names for sorting (ORDER BY) must be hard-coded or strictly validated in Python before inserting into the query. You cannot pass sorting column names as parameters to a SQL Server driver.
   2. You must initialize your connection with as_dict=True so pymssql yields row results as dictionaries rather than tuples, mapping perfectly to the JSON key layout required by the frontend.

## 1. The Python Backend (pymssql + Flask)
This approach executes two distinct SQL queries: one to retrieve the overall count matching the search criteria, and another targeting the subset of records to paginate.

~~~python
from flask import Flask, jsonify, request, render_templateimport pymssql

app = Flask(__name__)
ConfigurationDB_CONFIG = {
    "server": "localhost",
    "user": "your_username",
    "password": "your_password",
    "database": "your_database"
}

def get_db_connection():
    # Returns a connection object
    return pymssql.connect(**DB_CONFIG)

@app.route('/')def index():
    return render_template('table.html')

@app.route('/api/data')def get_data():
    # 1. Parse incoming parameters from Bootstrap Table
    limit = int(request.args.get('limit', 10))
    offset = int(request.args.get('offset', 0))
    search = request.args.get('search', '')
    sort_column = request.args.get('sort', 'id')
    sort_order = request.args.get('order', 'asc')

    # 2. Strict Whitelist validation for column sorting to prevent SQL injection
    ALLOWED_COLUMNS = {'id', 'name', 'category'}
    if sort_column not in ALLOWED_COLUMNS:
        sort_column = 'id'
        
    if sort_order.lower() not in {'asc', 'desc'}:
        sort_order = 'asc'

    # 3. Base WHERE clause logic for search filtering
    where_clause = ""
    query_params = []
    
    if search:
        # Using %s placeholders for standard pymssql parameter binding
        where_clause = "WHERE name LIKE %s OR category LIKE %s"
        search_param = f"%{search}%"
        query_params.extend([search_param, search_param])

    # Connect to the Database using context managers
    with get_db_connection() as conn:
        # Crucial: as_dict=True changes rows from tuple formats to dictionary objects
        with conn.cursor(as_dict=True) as cursor:
            
            # --- QUERY 1: Fetch total matching records (needed by Bootstrap Table) ---
            count_sql = f"SELECT COUNT(*) AS total_count FROM items {where_clause}"
            cursor.execute(count_sql, tuple(query_params))
            total_rows = cursor.fetchone()['total_count']

            # --- QUERY 2: Fetch paginated dataset ---
            # SQL Server requires an ORDER BY clause when utilizing OFFSET/FETCH pagination
            data_sql = f"""
                SELECT id, name, category 
                FROM items 
                {where_clause} 
                ORDER BY {sort_column} {sort_order}
                OFFSET %s ROWS FETCH NEXT %s ROWS ONLY
            """
            
            # Append offset and limit to our parameter binding payload
            pagination_params = query_params + [offset, limit]
            cursor.execute(data_sql, tuple(pagination_params))
            paginated_rows = cursor.fetchall()

    # 4. Construct JSON API payload mapping to Bootstrap Table
    return jsonify({
        "total": total_rows,
        "rows": paginated_rows
    })

if __name__ == '__main__':
    app.run(debug=True)
~~~

## Frontend
The HTML configuration matches the structure used in the previous example. Make sure the table data-field mappings match your database column names (id, name, category).

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>pymssql Server Pagination</title>
    <link rel="stylesheet" href="https://jsdelivr.net">
    <link rel="stylesheet" href="https://jsdelivr.net">
</head>
<body class="p-5">
    
    <h2>SQL Server Paginated Table (via pymssql)</h2>
    
    <table
      id="table"
      data-toggle="table"
      data-url="/api/data"
      data-side-pagination="server"
      data-pagination="true"
      data-search="true"
      data-sort-name="id"
      data-sort-order="asc"
      data-query-params="queryParams">
      <thead>
        <tr>
          <th data-field="id" data-sortable="true">ID</th>
          <th data-field="name" data-sortable="true">Item Name</th>
          <th data-field="category" data-sortable="true">Category</th>
        </tr>
      </thead>
    </table>

    <script src="https://jsdelivr.net"></script>
    <script src="https://jsdelivr.net"></script>
    <script src="https://jsdelivr.net"></script>

    <script>
        function queryParams(params) {
            return {
                limit: params.limit,
                offset: params.offset,
                search: params.search,
                sort: params.sort,
                order: params.order
            };
        }
    </script>
</body>
</html>
~~~

## Important Architecture Takeaways:

* The Tuple Transformation: When passing values to cursor.execute(), query_params must be cast to a tuple via tuple(pagination_params). Passing raw Python lists natively can throw syntax or connection exceptions in pymssql.
* String Formatting Security: Never inject frontend data parameters using f"{sort_column}" directly into your query strings without validation. The snippet uses an explicit ALLOWED_COLUMNS lookup array to avoid blind SQL Injection risks.

