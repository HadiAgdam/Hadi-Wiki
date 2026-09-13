


```c#

string str;

switch(a)
{
	case 1:
		str = "one"
		break;
	
	case 2:
		str = "two";
		break;
		
	default:
		str = "Unknown";
		break;
}


```


Another way:

```c#
string str = a switch
{
	1 => "one",
	2 => "two",
	_ => "Unknown"
}
```
