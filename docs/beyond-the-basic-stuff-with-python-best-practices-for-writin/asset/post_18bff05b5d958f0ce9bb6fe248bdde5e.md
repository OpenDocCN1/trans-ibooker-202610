
commands (continued)
md and mkdir (make folders), 34
move and mv (move files), 32
mv (rename files and folders), 32–33
rd and rmdir (delete folders), 34–35
ren (rename files and folders), 32–33
rm (delete files and folders), 33–34
running from Python programs, 27
shortened names, 32
where (find programs), 35
which (find programs), 35
wildcard characters, 28–29
commented-out code, 74–75
comments
best practices, 197
myth about, 83–84
using, 182–188
using with type hints, 196
commit history, rewriting in Git, 220
commit log, viewing in Git, 216–217
commits, rolling back in Git, 218–220
comparing objects, 154–155
comparison operators. See also sequence comparisons
chaining with assignment operators, 103, 105
function form of, 333
comparisons, making, 94–95
__complex__() numeric dunder method, 328
composition vs. inheritance, 299–301
conditional expressions, ternary operator, 101–102
containers, defined, 119–120
Cookiecutter, using to create projects, 200–202
copy and cp commands, 31
copy.copy() and copy.deepcopy(), using with mutable values, 140–142
copying
files and folders, 31
mutable values, 140–142
cProfile profiler, 154, 228–230
CPU, instruction set of, 129
CPython implementation, 108
CTRL key. See keyboard shortcuts
cwd (current working directory), 19–20, 31

## D
dash (-) using with command line arguments, 25
data, validating with setters, 319
data types
defined, 276
and return values, 177–178
dead code, 74–75
decimal.Decimal(), passing integers to, 148–149
decrement and increment operators, 156–157
default arguments, setting for parameters, 142–143, 165–166
default values, using collections.defaultdict for, 99–100
deleting
files and folders, 33–34
files from repo, 214–215
folders, 34–35
items from list, 134–140
and moving files in repo, 215–216
derived class, relationship to inheritance, 296
deterministic function, 173. See also functions
dictionaries
get() and setdefault() methods, 104
key-value pairs, 118
Python mailing list discussion, 131
setting type hints for, 195–196
using, 98–101
using as default arguments, 143–144
diff program, seeing changes with, 211–212
dir command, using, 30
dir /s command, 31
displays and literals, 110
__divmod__() numeric dunder method, 327
docs folder, contents of, 200
docstrings
defined, 182
summary, 197
using, 188–190
dollar sign ($), using in macOS, 23
doskey program, 28
dot (.), using with commands, 31
double dash (--), using with command line arguments, 25
double-free bugs, 109
double underscore (__), using in dunder methods, 322. See also underscore (_)
dunder methods. See also methods; OOP (object-oriented programming)
comparison, 332–337
defined, 120
numeric, 325–328
in-place augmented assignment, 330–332
reflected numeric, 328–330
string representation, 323–325
using, 322–323
duplicate code, 70–71
dynamic typing, 190

## E
Easter egg, The Zen of Python as, 88
encapsulation, explained, 307–308
encoding definition, using with magic comment, 187–188
engine vs. library vs. framework vs. SDK vs. API, 130
enumerate() vs. range(), 92–93, 104
environment setup, process of, 17, 42
environment variables and PATH, 35–39
__eq__() comparison dunder method, 336
Equal operation, 336
equality (==) operator, using with None, 94–95
error codes, returning, 178–179
error messages
and except blocks, 79–80
getting help with, 11
parsing, 15
tracing, 178
understanding, 4–8
errors, preventing with linters, 8–9
exceptions
catching, 79–80
raising, 4, 90, 178–179
RecursionError, 318–319
explanatory comments, 184–185
expressions vs. statements, 122–123

## F
False and True keywords, 158–159
FAQs (Frequently Asked Questions), 10
filenames, as command line arguments, 25
filenames and folders, matching with wildcards, 28–29
file paths, specifying, 20–21
files
copying, 31
deleting, 33–34
moving, 32
renaming, 32–33
filesystem, 18–21, 42
filtering with list comprehensions, 175–176
find command, 31
find feature, accessing, 64, 67
find() string method, error related to, 178
finding programs, 35
first-class objects and callables, 121–122. See also objects
flag arguments, myth about, 82
__float__() numeric dunder method, 328
floating-point numbers, accuracy of, 147–149, 151
__floor__() numeric dunder method, 328
__floordiv__() numeric dunder method, 327
folders
adding to PATH on macOS and Linux, 39
adding to PATH on Windows, 38–39
as command line arguments, 25
copying, 31
deleting, 33–34
in filesystem, 18
folders (continued)
home directory, 19
listing contents of, 30
making, 34
moving, 32
renaming, 32–33
folders and filenames, matching with wildcards, 28–29
for expressions, including in list comprehension, 79
for loops
in big O analysis, 240
getting index and value, 104
and lists, 134–140
versatility of, 125
form, filling out, 276–278
format() string method, 96–97
formatting for readability, 58
formatting strings, 96–97
forward slash (/)
purpose of, 18
using with command line arguments, 25
Four-in-a-Row tile-dropping game
output, 259–260
source code, 260–264
summary, 271–272
writing code, 264–271
frame object, explained, 5
frame summary, explained, 5
framework vs. library vs. SDK vs. engine vs. API, 130
f-strings, formatting strings with, 96–97
functional programming
higher-order functions, 174
lambda functions, 174–175
list comprehensions, 175–176
mapping and filtering, 175–176
side effects, 172–174
function calls, order in big O, 242–243
functions. See also deterministic function; higher-order functions; nondeterministic function; pure function; variadic functions; wrapper functions
default arguments, 165–166
vs. methods, 82, 124
names, 162
parameters and arguments, 165–172
passing arguments to, 166–167
and return statements, 80–81
size trade-offs, 162–165
and try statements, 81–82
using default arguments with, 142–143

## G
garbage collection, 109, 226
__ge__() comparison dunder method, 337
get(), using with dictionaries, 98–101
getPlayerMove() function, 163–165
getters and setters, 315, 318
Git. See also repo
adding files to track, 208–209
command line tool, 207
commits and repos, 200, 206–207
committed state, 204
committing changes, 210–214
configuring username and email, 203
deleting files from repo, 214–215
as distributed version control system, 206
frequency of committing changes, 213–214
ignoring files in repo, 209–210
installing, 202–204
keeping track of file status, 204–206
log command, 216–217
modified state, 204
recovering old changes, 217–220
renaming and moving files in repo, 215–216
rewriting commit history, 220
running status with watch command, 207
staged state, 205–206
storing private information in, 220
viewing changes before committing, 211–212
viewing commit log, 216–217
workflow, 204–206
git add command, 223
git clone command, 223
git commit command, 223
git diff, using, 211–213
git filter-branch command, 220
git init command, 223
GitHub and git push command, 221–223
glob patterns, explained, 29
global variables, myth about, 82–83
glossary, accessing, 108, 131
GrandchildClass, creating, 294–295
Greater Than operation, 337
Greater than or Equal operation, 337
__gt__() comparison dunder method, 337
GUI (graphical user interface), 22
GUI Git tools, installing, 203–204

## H
hash mark (#)
using with comments, 183
using with docstrings, 188
hashes, defined, 117–119
--help command line argument, 25–26
help with programming, asking for, 9–14
higher-order functions, 174. See also functions
home directory, 19
Homebrew, installing and configuring, 213
horizontal spacing, 47–51
Hungarian notation, 63

## I
id() function, calling, 111, 154
identifiers, defined, 59
identities, defined, 111–114
IEEE 754 standard and floating-point numbers, 147–148
if statement as clause header, 124
immutable and mutable objects, 114–117, 144
in operator, using with values of variables, 105
increment and decrement operators, 156–157
indentation, using space characters for, 47–48. See also significant indentation
index operator ([]), using, 117–118
index() string method, exception related to, 179
indexes, defined, 117–119
inequality != operators, avoiding chaining, 149–150
inheritance. See also classes; multiple inheritance; OOP (object-oriented programming)
base classes, 296
best practice, 308–309
class attributes, 306–307
class methods, 304–306
vs. composition, 299–301
creating child classes, 294
derived classes, 296
downside, 301–303
explained, 293
isinstance() and issubclass() functions, 303–304
MRO (method resolution order), 310–312
overriding methods, 296–297
static methods, 306–307
subclasses, 296
super classes, 296
super() function, 297–299
__init__(), and self, 280–282
__init__.py file and packages, 121
inline comments, 183–184
in-place augmented assignment dunder methods, 330–332
installing
Black code formatting tool, 54
Git, 202–204
Homebrew, 213
Meld for Linux, 213
Mypy, 192–193
Pyflakes, 9
tkdiff, 213
