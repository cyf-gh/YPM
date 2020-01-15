# YPM (Yuji Protocol Message)

YPM (Yuji Protocol Message) is a simple protocol structure, used for lightweight net or application message usage.

## Out-of-the-box

Example
```
$operation_name$@$param1$,$param2$,$param3$,...
```

$operation_param$ can be omitted, so the message below

```
get_info@
```

is acceptable.

## Tore

Tore features have been implemented in [GoYPM](https://github.com/cyf-gh/GoYPM) 

#### Reserved Keys

'@' and ',' and '\\' is reserved, and any other '@' or ',' should be escaped like

```
send_mail_address_and_note[@]cyf-ms@hotmail.com,that is\, nothing,C:\\,D:\,F:\\\\
```

above will be split into

```
send_mail_address_and_note			# operation name
cyf-ms@hotmail.com				 	# parament 1
that is, nothing					# parament 2
C:\								   # parament 3
D:\									# parament 4
F:\\								# parament 5
```

#### About '@'

 The '@' is wrapped if another '@' occurs, finally operation name and paraments
  is split by the most wrapped '@', like

```
send_wrap_god[[[@]]][@],[[@]],[@]
```

will be split into

```
send_wrap_god				# parament 1
[@],[[@]],[@]				# parament 2
```

and package below is ambiguous so is illegal meanwhile 

```
do_an_error[@],[@],@
```

#### About ',' and '\\'

```
','	->	'\,'
'\'	->	'\\'
```

##### What should I do if there's a ',' in one parament

Like

```
that is nothing
```

````
do_foo@that is\, nothing,true,true
````

will be spilt into 

```
[0] that is\, nothing
[1] true
[2] true
```

##### What should I do if there's a '\,' in one parament

Like

```
C:\
```

```
give_path@C:\\,true          -> [0] C:\		[1]true
```

so if you input

```
give_path@C:\\\,true		-> [0] C:\,true
```

like

```
a\\\\\a\\ -> a\\\\\a\\\\,
\\\ -> \\\\\\,
a\\\\\a\\, -> a\\\\\a\\\\\,,
```

if  '\\' before ',' is odd, the ',' will in the sentence, otherwise it will be spilt into two.

