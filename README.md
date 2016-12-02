
OGS.HOCON
=========
OGS.HOCON dotNet library based on the same HOCON notation described in [typesafe HCOCN](https://github.com/typesafehub/config/blob/master/HOCON.md).

Current implementation is not full support HOCON specification, follow link [LIMITATION.md](LIMITATION.md) to check current limitations.

**Notation example:**

# base.conf
isbase true;


#test.conf 

include "base.conf"

name    test

port    8000

ip:"127.0.0.1";

ip2=127.0.0.1;

ips [1,2,3,4]

item
{
    subItem{
        name subitem_v
    }
}

copyItem :${item} {
    cName cname;

    cPort 2000

    subItem{
        name override;
    }
}


copyItem2 :$(item) {
    cName cname;

    cPort 2000

    subItem{
        name override;
    }
}


copyItem3 :$item {
    cName cname;

    cPort 2000

    subItem{
        name override;
    }
}

item1
{

	path "11"
}

item2
{
	path "22"
}

item3
{
	path "33"
	param "aa"
}

watch [	$item1, $item2, $item3 ]

### How to use
**Using HOCON notation:**    
```csharp
 var reader = new DictionaryReader(item =>
{
    var content = File.ReadAllText(item);

    content = new Regex("\t+").Replace(content, " ");
    content = new Regex("\r\n{").Replace(content, "{");
    content = new Regex("( )*{( )*").Replace(content, "{");
    content = new Regex("( )*=( )*").Replace(content, "=");
    content = new Regex("( )*:( )*").Replace(content, ":");

    return content;
});

reader.Read("test.conf");

 MessageBox.Show(reader.Source["isbase"].ToString());

foreach (var item in (List<object>)reader.Source["ips"])
{
    MessageBox.Show(item.ToString());
}

MessageBox.Show(reader.Source["item.subItem.name"].ToString());

var objs = ConfigUtils.Read<List<dynamic>>("watch");

foreach (dynamic o in objs)
{
MessageBox.Show(o.path.ToString());
}
```


​    
