# MetInfo 5.3.17 PV statistics sitepage XSS #


## 1.Technical Description: ##

locate in /include/stat/stat.php line:7

	if($type=='para' && $met_stat){
  	if (!empty($_SERVER['HTTP_CLIENT_IP'])){
    	$ip=$_SERVER['HTTP_CLIENT_IP'];
  	}elseif(!empty($_SERVER['HTTP_X_FORWARDED_FOR'])){
    	$ip=$_SERVER['HTTP_X_FORWARDED_FOR'];
  	}else{
    	$ip=$_SERVER['REMOTE_ADDR'];
	}

Then we can control the variable $type, so we can control the $_SERVER['HTTP_CLIENT_IP'] or $_SERVER['HTTP_X_FORWARDED_FOR'] 
according to the HTTP header.

locate in /include/stat/stat.php line:59 and 72

	$metinfo = .....
	murl+='&ip={$ip}';
	.....
	.....
	echo $metinfo;

so we can use $ip to create a XSS when echo $metinfo;

## 2.PoC ##

Use the Firefox plug-in "modify header" to modify our HTTP HEADER X-FORWARD-FOR to "<script>alert(123)</script>"

and when we use

> http://127.0.0.1/include/stat/stat.php?type=para


![](https://github.com/phantom0301/vulns/blob/master/111.png)

![](https://github.com/phantom0301/vulns/blob/master/222.png)

The XSS will be triggered
