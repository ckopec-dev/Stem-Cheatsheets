# Perl Tutorial

## Table of Contents

1. Introduction to Perl
2. Installing Perl
3. Your First Perl Script
4. Perl Syntax Basics
5. Scalars, Arrays, and Hashes
6. Operators and Control Flow
7. Subroutines
8. Regular Expressions (Perl’s Superpower)
9. File Handling
10. Working with Modules (CPAN)
11. Object-Oriented Perl
12. Error Handling and Debugging
13. Practical Examples
14. Best Practices
15. Next Steps

---

## 1. Introduction to Perl

Perl (Practical Extraction and Report Language) is a high-level, interpreted programming language known for its powerful text-processing capabilities, flexibility, and extensive ecosystem of modules.

Perl is widely used for:

* System administration
* Text processing and log parsing
* Web development
* Bioinformatics
* Network programming

---

## 2. Installing Perl

### Windows

* Download Strawberry Perl or ActivePerl
* Verify installation:

```bash
perl -v
```

### Linux

Most distributions come with Perl preinstalled.

```bash
sudo apt install perl
perl -v
```

### macOS

Perl is usually installed by default.

```bash
perl -v
```

---

## 3. Your First Perl Script

Create a file called `hello.pl`:

```perl
#!/usr/bin/perl
use strict;
use warnings;

print "Hello, world!\n";
```

Run it:

```bash
perl hello.pl
```

---

## 4. Perl Syntax Basics

* Statements end with semicolons
* Variables start with special symbols
* Blocks use `{ }`
* Comments start with `#`

```perl
# This is a comment
print "Perl is fun!\n";
```

---

## 5. Scalars, Arrays, and Hashes

### Scalars (`$`)

Hold single values (strings, numbers).

```perl
my $name = "Chris";
my $age = 30;
print "$name is $age years old\n";
```

### Arrays (`@`)

Ordered lists.

```perl
my @colors = ("red", "green", "blue");
print $colors[0];
```

### Hashes (`%`)

Key-value pairs.

```perl
my %person = (
  name => "Chris",
  age  => 30
);
print $person{name};
```

---

## 6. Operators and Control Flow

### Conditionals

```perl
if ($age > 18) {
  print "Adult\n";
} else {
  print "Minor\n";
}
```

### Loops

#### for

```perl
for (my $i=0; $i<5; $i++) {
  print "$i\n";
}
```

#### foreach

```perl
foreach my $color (@colors) {
  print "$color\n";
}
```

#### while

```perl
while (<STDIN>) {
  chomp;
  print "You typed: $_\n";
}
```

---

## 7. Subroutines

```perl
sub greet {
  my ($name) = @_;
  return "Hello, $name";
}

print greet("Chris");
```

---

## 8. Regular Expressions (Perl’s Superpower)

### Matching

```perl
if ($text =~ /hello/i) {
  print "Found hello";
}
```

### Substitution

```perl
$text =~ s/cat/dog/g;
```

### Capture Groups

```perl
if ($email =~ /(\w+)@(\w+\.\w+)/) {
  print "User: $1 Domain: $2";
}
```

---

## 9. File Handling

### Reading a file

```perl
open my $fh, '<', 'data.txt' or die $!;
while (my $line = <$fh>) {
  print $line;
}
close $fh;
```

### Writing a file

```perl
open my $fh, '>', 'out.txt' or die $!;
print $fh "Hello file\n";
close $fh;
```

---

## 10. Working with Modules (CPAN)

Install a module:

```bash
cpan install JSON
```

Use it:

```perl
use JSON;
my $data = decode_json('{"a":1}');
print $data->{a};
```

---

## 11. Object-Oriented Perl

```perl
package Animal;

sub new {
  my ($class, $name) = @_;
  return bless { name => $name }, $class;
}

sub speak {
  my ($self) = @_;
  print "$self->{name} makes a sound\n";
}

1;
```

Use it:

```perl
use Animal;
my $dog = Animal->new("Rex");
$dog->speak();
```

---

## 12. Error Handling and Debugging

```perl
use warnings;
use strict;

open my $fh, '<', 'nofile.txt' or die "Cannot open file: $!";
```

Debugging:

```bash
perl -d script.pl
```

---

## 13. Practical Examples

### Log File Parser

```perl
while (<>) {
  print if /ERROR/;
}
```

### Simple Web Request

```perl
use LWP::Simple;
my $content = get("https://example.com");
print $content;
```

---

## 14. Best Practices

* Always use `strict` and `warnings`
* Use meaningful variable names
* Modularize code
* Comment complex logic
* Prefer CPAN modules over reinventing the wheel

---

## 15. Next Steps

* Learn Modern Perl practices
* Explore Moose or Moo for OOP
* Study advanced regex
* Build real tools: scrapers, automation scripts, servers

---

## Resources

* perldoc perlintro
* perldoc perlre
* [https://www.perl.org](https://www.perl.org)
* [https://metacpan.org](https://metacpan.org)

