goconfig
========
##About
goconfig is a easy-use comments-support configuration file parser for the Go Programming Language which provides a structure similar to what you would find on Microsoft Windows INI files.

The configuration file consists of sections, led by a "*[section]*" header and followed by "*name:value*" entries; "*name=value*" is also accepted. Note that leading whitespace is removed from values. The optional values can contain format strings which refer to other values in the same section, or values in a special DEFAULT section. Comments are indicated by ";" or "#"; comments may begin anywhere on a single line.

##Features
- It simplified operation processes, easy to use and undersatnd; therefore, there are less chances to have errors. 
- It uses exactly the same way to access a configuration file as you use windows APIs, so you don't need to change your code style.
- It supports configuration file with comments each section or key which all the other parsers don't support!!!!!!!
- It Compiles!! It works with go version 1 and later.

##Example(Comments Support!!!!)
###Config.ini
	; Google
	google=www.google.com
	search: http://%(google)s
	
	; Here are Comments
	; Second line
	[Demo]
	# This symbol can also make this line to be comments
	key1=Let's us GoConfig!!!
	key2=test data
	key3=this is based on key2:%(key2)s

	[What's this?]
	; Not Enough Comments!!
	name=try one more value ^-^
###Code Fragment
```go
	// Open and read configuration file
	c, err := GoConfig.LoadConfigFile("Config.ini")
	
	// GetValue
	value, _ := c.GetValue("Demo", "key1") // return "Let's use GoConfig!!!"

	// GetComments
	comments := c.GetKeyComments("Demo", "key1") // return "# This symbol can also make this line to be comments"

	// SetValue
	c.SetValue("What's this?", "name", "Do it!") // Now name's value is "Do it!"
	search, _ := c.GetValue(DEFAULT_SECTION, "search")
	c.SetValue(DEFAULT_SECTION, "path", search)
	key3, _ := c.GetValue("Demo", "key3")
	c.SetValue("Demo", "key3", key3)
	
	// You can even edit comments in your code
	c.SetKeyComments("Demo", "key1", "")
	c.SetKeyComments("Demo", "key2", "comments by code without symbol")
	c.SetKeyComments("Demo", "key3", "# comments by code with symbol")

	// Don't need that key any more? Pass empty string "" to remove! that's all!'
	c.SetValue("What's this?", "name", "") // If your key was removed, its comments will be removed too!
	c.SetValue("What's this?", "name_test", "added by test")

	// Finally, you need save it
	SaveConfigFile(c, "Config_test.ini")
```
##Installation
	go get github.com/Unknwon/goconfig
##More Information
- All characters are CASE SENSITIVE, BE CAREFULL!
- If you use other operation systems instead of windows, you may want to change global variable [ LineBreak ] in conf.go, replace it with suitable characters, default value "\r\n" is for windows only. You can also use "\n" in all operation systems because I use "\n" as line break, it may look strange when you open with Notepad.exe in windows, but it works anyway. 

##References
- [goconf](http://code.google.com/p/goconf/)
- [robfig/config](https://github.com/robfig/config)
- [Delete an item from a slice](https://groups.google.com/forum/?fromgroups=#!topic/golang-nuts/lYz8ftASMQ0)