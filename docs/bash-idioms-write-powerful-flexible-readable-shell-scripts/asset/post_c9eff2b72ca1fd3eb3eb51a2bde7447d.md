
libraries, 94
line continuation character, 27, 130-131
linters, 134
listing arguments, 74
lists, 61-65
  accidental assignment in, 60
  common operations, 61
  declaring, 61
  example code, 62-64
  in for loops, 13-14
  hashes vs., 60-61
  resources for information, 72
  terminology, 59
local variables, 54, 110
logger tool, 111
logging, style guidelines for, 129
long options, 76-79
loops
  for loops
    developing and testing, 15
    explicit values in, 11-12
    hashes in, 13-14
    implicit values, 10
    lists in, 13-14
    quotation marks and blank spaces in, 14-15
    types of, 9-10
  infinite loops, 9
  until loops, 17
  while loops, 16
ls command, parsing options, 21-23

M
Macs, bash on, ix-ix
man page, 2
mapfile command, 86-89
mcd function, 109
minus sign (-)
  in conditional substitutions, 38
  in here-documents, 98-99
  for options, 74
  with shebang, 95
modified values, conditional substitutions for, 39
mssh (MultiSSH), 16
multiple hosts, executing commands across, 16
multixterm, 16
mussh, 16

N
naming conventions, 127-128, 139
navigating command-line history, 110-111
null statement, : (colon) as, 3
null variables, 38

O
old files, rotating, 114-115
operator precedence for conditionals, 4-5
OPTIND -1, 76
options, 73
  (see also arguments)
  DEBUG variable, 82
  help, 79-82
  lastpipe, 86
  long options, 76-79
  parsing, 75-76
  syntax, 74-75
  VERBOSE variable, 82
  version, 83
OR (||)
  as else statement, 3
  operator precedence, 4-5
overrides in style guide, 124

P
PAC Manager, 16
parameter expansion, 32-36
  additional modifiers for, 34-36
  basename shorthand, 32-33
  dirname/suffix removal, 34
  path/prefix removal, 33
parameters, 52-53
parentheses ()
  in arithmetic, 44-46
  in case statement, 20
  in function definition, 55
  in loops, 9
  subshells in, 4
  when not needed, 46-47
parsing
  arguments, 83
  command-line options with case statement, 21-23
  JSON, 112-113
  options, 75-76
pathname in variable references, removing, 33
pattern matching
in case statement, 20-21
consecutive patterns, 28-29
regular expressions, 21
pause command, 106
pconsole, 16
percent sign (%) in variable references, 34
Perl POD, 115
plus sign (+) in conditional substitutions, 38
POSIX, viii, ix
  arrays in, 60
  output with printf function, 56
prefixes in variable references, removing, 33
printenv command, 121
printf -v command, 12
printf function, 55-58
  date and time with, 57
  debugging with, 58
  POSIX output, 56
  reusing output, 58
process list, grepping, 113
process substitution, 92
prompts, 102-104
PROMPT_COMMAND, 104
PS0 prompt, 102
PS1 prompt, 103
PS2 prompt, 103
PS3 prompt, 103
PS4 prompt, 103
push, 61

Q
question mark (?) in pattern matching, 20
quotation marks (")
  in arguments, 74
  in for loops, 14-15
style guidelines for, 129-130, 140
variable interpolation, 31

R
random variables, 39-40
read command, 85-86, 104-106
readability
  definition of, vii
  of style guide, 124-126, 137
reading files
  $IFS (internal field separator), 89-92
  via brute force, 89
  with mapfile command, 86-89
  with process substitution, 92
with read command, 85-86
readline library, 110-111
redirection in function definition, 55
redirection operators, 49
referencing
  arguments, 73-74
  variables (see variable references)
regular expressions, 21, 34, 48
removing
dirname/suffix in variable references, 34
files from directories, 93
old files, 114-115
path/prefix in variable references, 33
return status, 45
return values, 53-54
reusing printf output, 58
revision control, ix, 124
right angle bracket (>) in single/double brackets, 49
rotating old files, 114-115

S
select command, 107
semicolon (;) in compound commands, 4
seq command, 11-12
set commands, 96, 121
shebang (#!), 94-95
shell patterns in variable references, 34
shell scripts
  executing on signal, 97-98
  exit codes, 96
  files as, 94-95
  for loops in, 10
  interactive shells, 99
  strict mode, 96
  template for, 132-133, 142
Shell Style Guide, 133
shellcheck, 134
signals, executing scripts on, 97-98
simplicity in style guide, 123
single brackets ([ ])
  double brackets ([[]]) vs., 6, 47-49
  in pattern matching, 20
single quotation marks (')
  avoiding variable interpolation, 31
  style guidelines for, 129-130, 140
slash (/) in variable references, 35
sleep command, 44
sort command, 71
spaces, variable assignments and, 46
STDERR, redirection to, 55
strict mode, 96
string length of the value, 32
style guidelines, 123-124
  bash Idioms style guide, 137-142
  for comments, 126-127, 138
  for functions, 128-129, 139
  for layout of code, 130-131, 141
  linters with, 134
  list of alternative style guides, 133-134
  for logging, 129
  miscellaneous guidelines, 132, 142
  for naming, 127-128, 139
  for quotation marks, 129-130, 140
  readability of, 124-126, 137
  script template, 132-133, 142
  for syntax, 131, 141
subscripts, 59-60
subshells, 4
substrings, extracting from variable references, 35
suffixes in variable references, removing, 34
syntax, style guidelines for, 131, 141

T
template for scripts, 132-133, 142
test command, 47
testing for loops, 15
time
  with printf function, 57
  in prompt, 104
time zones in prompt, 104
trailing dash (-), with shebang, 95
trap command, 97-98
two dashes (--), long options, 77

U
unalias command, 108
unit testing, 122
unset command, 64
unset variables, 38
until loops, 17
user input, 104
  pause command, 106
  read command, 104-106
select command, 107

V
variable interpolation, 56
variable references
  $RANDOM, 39-40
  command substitution, 40
  conditional substitutions, 38-39
    for comma-separated lists, 38
    default values and, 38
    for modified values, 39
  dynamic scoping, 55
  integers in, 46-47
  local variables, 54, 110
  null variables, 38
  parameter expansion, 32-36
    additional modifiers for, 34-36
    basename shorthand, 32-33
    dirname/suffix removal, 34
    path/prefix removal, 33
  readability of, 124
  spaces and, 46
  syntax, 31-32
  unset variables, 38
VERBOSE variable, 82
version compatibility, 60
version option, 83
vertical bar (|) in pattern matching, 20
viewing JSON in Firefox, 113

W
while loops, 16
wildcards, 20
Windows, bash on, viii
word count example (hashes), 69-71
word splitting, 89
wrapper scripts, example of, 23-28

X
xtrace, 121

Z
zero-padding of expanded numeric values, 11-12
Zsh, ix, 40, 60

## About the Authors

Carl Albing is a professor, researcher, and software engineer with a breadth of industry experience. A coauthor of O’Reilly’s bash Cookbook and Cybersecurity Ops with bash, as well as the author of O’Reilly’s “Great bash” video, Carl has worked in software (using bash and many other languages) for companies large and small, across a variety of industries. He has a BA in mathematics, a master’s in international management (MIM), and a PhD in computer science.

JP Vossen has been working with computers since the early 80s and has been in the IT industry since the early 90s, specializing in information security since the late 90s. He’s been fascinated with scripting and automation since he first understood what an autoexec.bat was, and was delighted to discover the power and flexibility of bash and GNU on Linux in the mid-90s. He has previously written for Information Security Magazine and SearchSecurity, among other publications. On those few occasions when he’s not in front of a computer, he is usually taking something apart, putting something together, or both.

## Colophon

The animal on the cover of bash Idioms is a harp shell (Harpa articularis). Commonly known as the articulate harp shell, it is a species of sea snail in the family Harpidae. The Harpidae family consists of about 55 species, the majority of which are found in the Indo-Pacific region, with only two of the species found in the coastal waters along the Baja Peninsula. This particular species, Harpa articularis, is found in shallow waters in the Indian Ocean and South Pacific, including as far as Fiji and the Australian coast. This species is known for the intricate scalloped patterns and colors of its shells, which range between 50 to 110 mm in size.

The Harpidae family can be identified by their smooth polished exterior. There are strong ribs running along the length of the shell, and the aperture is large and flared with a notch at the bottom. They do not have an operculum (a sheet-like structure that acts as a trapdoor for the snail inside, preventing the animal from drying up). Instead, the animal’s large foot extends well beyond the edge of the shell.

Harp shells are nocturnal predators. They bury themselves in sand during the day and emerge at night to feed on crabs and shrimp. They feed by covering their prey with their foot and enveloping it in mucus. In turn, they can fall prey to larger crabs, fish, and predatory mollusks. In order to defend themselves, the harp shell uses a very interesting defense technique: it has the ability to amputate a rear portion of its foot, which continues to wiggle and distract the predator while the harp shell itself crawls away.
