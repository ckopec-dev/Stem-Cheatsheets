# Unity3D Roguelike Game Tutorial

## Introduction

This tutorial will guide you through creating a classic roguelike game in Unity3D. We'll build a dungeon crawler with procedural generation, turn-based combat, permadeath, and grid-based movement.

## Prerequisites

- Unity 2021.3 LTS or newer
- Basic C# knowledge
- Understanding of Unity fundamentals (GameObjects, Components, Prefabs)

## Project Setup

1. Create a new Unity 2D project
2. Set up the following folder structure:
   - Assets/Scripts
   - Assets/Prefabs
   - Assets/Sprites
   - Assets/Scenes

## Part 1: Grid System

The foundation of a roguelike is the grid-based world. Create `GridManager.cs`:

```csharp
using UnityEngine;
using System.Collections.Generic;

public class GridManager : MonoBehaviour
{
    public static GridManager Instance;
    
    public int width = 50;
    public int height = 50;
    public GameObject floorTile;
    public GameObject wallTile;
    
    private TileType[,] grid;
    
    public enum TileType
    {
        Floor,
        Wall
    }
    
    void Awake()
    {
        Instance = this;
        grid = new TileType[width, height];
    }
    
    public bool IsWalkable(int x, int y)
    {
        if (x < 0 || x >= width || y < 0 || y >= height)
            return false;
        return grid[x, y] == TileType.Floor;
    }
    
    public void SetTile(int x, int y, TileType type)
    {
        grid[x, y] = type;
    }
    
    public Vector3 GridToWorld(int x, int y)
    {
        return new Vector3(x, y, 0);
    }
}
```

## Part 2: Procedural Dungeon Generation

Create `DungeonGenerator.cs` to generate random dungeons:

```csharp
using UnityEngine;
using System.Collections.Generic;

public class DungeonGenerator : MonoBehaviour
{
    public int roomCount = 10;
    public int minRoomSize = 4;
    public int maxRoomSize = 10;
    
    private List<Room> rooms = new List<Room>();
    
    public class Room
    {
        public int x, y, width, height;
        
        public Room(int x, int y, int width, int height)
        {
            this.x = x;
            this.y = y;
            this.width = width;
            this.height = height;
        }
        
        public Vector2Int Center()
        {
            return new Vector2Int(x + width / 2, y + height / 2);
        }
        
        public bool Intersects(Room other)
        {
            return x < other.x + other.width &&
                   x + width > other.x &&
                   y < other.y + other.height &&
                   y + height > other.y;
        }
    }
    
    public void GenerateDungeon()
    {
        rooms.Clear();
        GridManager grid = GridManager.Instance;
        
        // Fill with walls
        for (int x = 0; x < grid.width; x++)
        {
            for (int y = 0; y < grid.height; y++)
            {
                grid.SetTile(x, y, GridManager.TileType.Wall);
                Instantiate(grid.wallTile, grid.GridToWorld(x, y), Quaternion.identity);
            }
        }
        
        // Generate rooms
        for (int i = 0; i < roomCount; i++)
        {
            int w = Random.Range(minRoomSize, maxRoomSize);
            int h = Random.Range(minRoomSize, maxRoomSize);
            int x = Random.Range(1, grid.width - w - 1);
            int y = Random.Range(1, grid.height - h - 1);
            
            Room newRoom = new Room(x, y, w, h);
            bool overlaps = false;
            
            foreach (Room room in rooms)
            {
                if (newRoom.Intersects(room))
                {
                    overlaps = true;
                    break;
                }
            }
            
            if (!overlaps)
            {
                CreateRoom(newRoom);
                
                if (rooms.Count > 0)
                {
                    ConnectRooms(rooms[rooms.Count - 1], newRoom);
                }
                
                rooms.Add(newRoom);
            }
        }
    }
    
    void CreateRoom(Room room)
    {
        GridManager grid = GridManager.Instance;
        
        for (int x = room.x; x < room.x + room.width; x++)
        {
            for (int y = room.y; y < room.y + room.height; y++)
            {
                grid.SetTile(x, y, GridManager.TileType.Floor);
                Instantiate(grid.floorTile, grid.GridToWorld(x, y), Quaternion.identity);
            }
        }
    }
    
    void ConnectRooms(Room room1, Room room2)
    {
        Vector2Int start = room1.Center();
        Vector2Int end = room2.Center();
        
        // Create L-shaped corridor
        CreateHorizontalTunnel(start.x, end.x, start.y);
        CreateVerticalTunnel(start.y, end.y, end.x);
    }
    
    void CreateHorizontalTunnel(int x1, int x2, int y)
    {
        GridManager grid = GridManager.Instance;
        
        for (int x = Mathf.Min(x1, x2); x <= Mathf.Max(x1, x2); x++)
        {
            grid.SetTile(x, y, GridManager.TileType.Floor);
            Instantiate(grid.floorTile, grid.GridToWorld(x, y), Quaternion.identity);
        }
    }
    
    void CreateVerticalTunnel(int y1, int y2, int x)
    {
        GridManager grid = GridManager.Instance;
        
        for (int y = Mathf.Min(y1, y2); y <= Mathf.Max(y1, y2); y++)
        {
            grid.SetTile(x, y, GridManager.TileType.Floor);
            Instantiate(grid.floorTile, grid.GridToWorld(x, y), Quaternion.identity);
        }
    }
    
    public Vector2Int GetRandomRoomPosition()
    {
        if (rooms.Count == 0) return Vector2Int.zero;
        return rooms[0].Center();
    }
}
```

## Part 3: Turn-Based System

Create `TurnManager.cs` to handle turn-based gameplay:

```csharp
using UnityEngine;
using System.Collections.Generic;

public class TurnManager : MonoBehaviour
{
    public static TurnManager Instance;
    
    private List<Actor> actors = new List<Actor>();
    private bool playerTurn = true;
    
    void Awake()
    {
        Instance = this;
    }
    
    public void RegisterActor(Actor actor)
    {
        if (!actors.Contains(actor))
            actors.Add(actor);
    }
    
    public void UnregisterActor(Actor actor)
    {
        actors.Remove(actor);
    }
    
    public void EndPlayerTurn()
    {
        playerTurn = false;
        StartCoroutine(ProcessEnemyTurns());
    }
    
    System.Collections.IEnumerator ProcessEnemyTurns()
    {
        foreach (Actor actor in actors)
        {
            if (actor != null && actor.CompareTag("Enemy"))
            {
                actor.TakeTurn();
                yield return new WaitForSeconds(0.1f);
            }
        }
        
        playerTurn = true;
    }
    
    public bool IsPlayerTurn()
    {
        return playerTurn;
    }
}
```

## Part 4: Actor System

Create `Actor.cs` as the base class for all entities:

```csharp
using UnityEngine;

public class Actor : MonoBehaviour
{
    public int health = 10;
    public int maxHealth = 10;
    public int attack = 2;
    public int defense = 0;
    
    protected int gridX;
    protected int gridY;
    
    protected virtual void Start()
    {
        TurnManager.Instance.RegisterActor(this);
        
        Vector3 pos = transform.position;
        gridX = Mathf.RoundToInt(pos.x);
        gridY = Mathf.RoundToInt(pos.y);
    }
    
    protected void OnDestroy()
    {
        if (TurnManager.Instance != null)
            TurnManager.Instance.UnregisterActor(this);
    }
    
    public virtual bool TryMove(int dx, int dy)
    {
        int newX = gridX + dx;
        int newY = gridY + dy;
        
        if (!GridManager.Instance.IsWalkable(newX, newY))
            return false;
        
        // Check for other actors
        Actor otherActor = GetActorAt(newX, newY);
        if (otherActor != null)
        {
            Attack(otherActor);
            return true;
        }
        
        gridX = newX;
        gridY = newY;
        transform.position = GridManager.Instance.GridToWorld(gridX, gridY);
        return true;
    }
    
    protected Actor GetActorAt(int x, int y)
    {
        Actor[] allActors = FindObjectsOfType<Actor>();
        foreach (Actor actor in allActors)
        {
            if (actor.gridX == x && actor.gridY == y && actor != this)
                return actor;
        }
        return null;
    }
    
    public virtual void Attack(Actor target)
    {
        int damage = Mathf.Max(1, attack - target.defense);
        target.TakeDamage(damage);
        Debug.Log($"{gameObject.name} attacks {target.gameObject.name} for {damage} damage!");
    }
    
    public virtual void TakeDamage(int damage)
    {
        health -= damage;
        
        if (health <= 0)
        {
            Die();
        }
    }
    
    protected virtual void Die()
    {
        Debug.Log($"{gameObject.name} has died!");
        Destroy(gameObject);
    }
    
    public virtual void TakeTurn()
    {
        // Override in subclasses
    }
}
```

## Part 5: Player Controller

Create `PlayerController.cs`:

```csharp
using UnityEngine;

public class PlayerController : Actor
{
    void Update()
    {
        if (!TurnManager.Instance.IsPlayerTurn())
            return;
        
        int dx = 0;
        int dy = 0;
        
        if (Input.GetKeyDown(KeyCode.W) || Input.GetKeyDown(KeyCode.UpArrow))
            dy = 1;
        else if (Input.GetKeyDown(KeyCode.S) || Input.GetKeyDown(KeyCode.DownArrow))
            dy = -1;
        else if (Input.GetKeyDown(KeyCode.A) || Input.GetKeyDown(KeyCode.LeftArrow))
            dx = -1;
        else if (Input.GetKeyDown(KeyCode.D) || Input.GetKeyDown(KeyCode.RightArrow))
            dx = 1;
        
        if (dx != 0 || dy != 0)
        {
            if (TryMove(dx, dy))
            {
                TurnManager.Instance.EndPlayerTurn();
            }
        }
    }
    
    protected override void Die()
    {
        Debug.Log("Game Over!");
        // Implement game over logic here
        base.Die();
    }
}
```

## Part 6: Enemy AI

Create `Enemy.cs`:

```csharp
using UnityEngine;

public class Enemy : Actor
{
    private Transform player;
    
    protected override void Start()
    {
        base.Start();
        player = GameObject.FindGameObjectWithTag("Player").transform;
    }
    
    public override void TakeTurn()
    {
        if (player == null)
            return;
        
        // Simple AI: move towards player if in range
        float distance = Vector2.Distance(transform.position, player.position);
        
        if (distance < 8f)
        {
            int dx = 0;
            int dy = 0;
            
            if (Mathf.Abs(player.position.x - transform.position.x) > 
                Mathf.Abs(player.position.y - transform.position.y))
            {
                dx = player.position.x > transform.position.x ? 1 : -1;
            }
            else
            {
                dy = player.position.y > transform.position.y ? 1 : -1;
            }
            
            TryMove(dx, dy);
        }
    }
}
```

## Part 7: Game Manager

Create `GameManager.cs` to tie everything together:

```csharp
using UnityEngine;

public class GameManager : MonoBehaviour
{
    public GameObject playerPrefab;
    public GameObject enemyPrefab;
    public int enemyCount = 15;
    
    void Start()
    {
        DungeonGenerator generator = GetComponent<DungeonGenerator>();
        generator.GenerateDungeon();
        
        // Spawn player
        Vector2Int playerPos = generator.GetRandomRoomPosition();
        Instantiate(playerPrefab, GridManager.Instance.GridToWorld(playerPos.x, playerPos.y), 
                   Quaternion.identity);
        
        // Spawn enemies
        for (int i = 0; i < enemyCount; i++)
        {
            Vector2Int pos = GetRandomFloorPosition();
            Instantiate(enemyPrefab, GridManager.Instance.GridToWorld(pos.x, pos.y), 
                       Quaternion.identity);
        }
        
        // Position camera
        Camera.main.transform.position = new Vector3(playerPos.x, playerPos.y, -10);
    }
    
    Vector2Int GetRandomFloorPosition()
    {
        int x, y;
        do
        {
            x = Random.Range(0, GridManager.Instance.width);
            y = Random.Range(0, GridManager.Instance.height);
        }
        while (!GridManager.Instance.IsWalkable(x, y));
        
        return new Vector2Int(x, y);
    }
}
```

## Setup Instructions

1. **Create Sprites**: Use simple colored squares for testing:
   - Floor: White square
   - Wall: Dark gray square
   - Player: Blue square
   - Enemy: Red square

2. **Create Prefabs**:
   - Create a GameObject with a SpriteRenderer for floor tiles
   - Create a GameObject with a SpriteRenderer for wall tiles
   - Create player prefab with PlayerController and a blue sprite
   - Create enemy prefab with Enemy component and a red sprite
   - Tag the player prefab as "Player"
   - Tag enemy prefabs as "Enemy"

3. **Scene Setup**:
   - Create an empty GameObject called "GameManager"
   - Add GridManager, DungeonGenerator, TurnManager, and GameManager scripts to it
   - Assign prefabs in the inspector
   - Set camera to Orthographic, size 10

4. **Configure Components**:
   - GridManager: Set width/height (50x50), assign floor/wall prefabs
   - DungeonGenerator: Configure room count and sizes
   - GameManager: Assign player/enemy prefabs, set enemy count

## Next Steps

To expand your roguelike:

- **Items & Inventory**: Create pickups, weapons, and equipment
- **Field of View**: Implement fog of war using raycasting
- **More Enemy Types**: Add different behaviors and stats
- **Level Progression**: Add stairs to generate new levels
- **Save System**: Implement save/load (contradicts permadeath, but useful for testing)
- **UI**: Add health bars, inventory display, message log
- **Hunger System**: Classic roguelike mechanic
- **Status Effects**: Poison, confusion, paralysis, etc.

## Common Roguelike Features

- **Permadeath**: Already implemented (no respawning)
- **Procedural Generation**: ✓ Dungeon generator
- **Turn-Based**: ✓ Turn manager system
- **Grid-Based**: ✓ Grid manager
- **Exploration**: Add items and secrets
- **Difficulty Curve**: Increase challenge with depth

This tutorial provides a solid foundation for a traditional roguelike. Experiment with different generation algorithms, enemy behaviors, and game mechanics to make it your own!