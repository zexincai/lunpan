<?php
/**
 * Created by PhpStorm.
 * User: KenChung
 * Date: 2018/10/19
 * Time: 17:55
 * Email: 120825579@qq.com
 */

require_once 'zfdmkjV1/app/config.php';
//require_once 'yzbtkjListenV1/util/IPUtil.php';
//require_once 'yzbtkjListenV1/util/PageUtil.php';
//require_once 'yzbtkjListenV1/util/HttpUtil.php';

$ip = IPUtil::getRealIpAddr ();
$name = !empty($_POST['name'])?$_POST['name']:'';
$phone = !empty($_POST['contact'])?$_POST['contact']:'';
$qq = !empty($_POST['qq'])?$_POST['qq']:'';
$business = !empty($_POST['profession'])?$_POST['profession']:'';
$hasLicense = !empty($_POST['license'])?$_POST['license']:'';
$isCertified = !empty($_POST['isCertified'])?$_POST['isCertified']:'';
$hasOA = !empty($_POST['hasOA'])?$_POST['hasOA']:'';

$device = CommonUtil::getDevicType ();
$source = !empty($_POST['source'])?'gw_'.$device.'_'.$_POST['source']:'gw_'.$device;
$browser = $_SERVER['HTTP_USER_AGENT'];
$userKey = SiteUserFunction::getUniqueKey ( '.zfdmkj.com', $ip, $browser );
$result = array (
		'userKey' => $userKey
);

if(empty($name)){
    $returnParam = array(
        'errorMsg' => '请填写名字',
    );
    echo PageUtil::setJsonReturn(-1 , $returnParam );
    exit;
}

if(empty($phone)){
    $returnParam = array(
        'errorMsg' => '请填写联系方式',
    );
    echo PageUtil::setJsonReturn(-2 , $returnParam );
    exit;
}


$IPProvince = IPUtil::getProvinceByIP($ip);

//$content = '【'.date('Y-m-d H:i:s').'】ip:'.$ip.' 名字:'.$name . ' 联系方式:'.$phone . ' qq:'.$qq. ' 行业：'.$business. ' 营业执照：'.$hasLicense. ' 公众号：'.$hasOA. ' 认证：'.$isCertified.' 省份：'.$IPProvince."\n";
//$content .= $browser.' '.$userKey;

$IPProvince = IPUtil::getProvinceByIP ( $ip );
$ret = UserAppletLogic::addUser ( $name, $phone, $qq, $source, $userKey, $IPProvince, $business, $hasLicense, '', $isCertified, $hasOA );
if (is_numeric ( $ret ['ret'] ) && $ret ['ret'] >= 0) {
	$result ['salesID'] = ! empty ( $ret ['salesID'] ) ? $ret ['salesID'] : 0;
	$result ['firstSubmitID'] = ! empty ( $ret ['firstSubmitID'] ) ? $ret ['firstSubmitID'] : 0;
	$result ['appletClass'] = $ret ['appletClass'];
	$result ['qrCodeUrl'] = '';
	$result ['waitNum'] = mt_rand ( 100, 150 );
	if (! empty ( $ret ['salesID'] ) && isset ( SalesLang::$QDQQUrl [$ret ['salesID']] )) {
		$result ['salesUrl'] = SalesLang::$QDQQUrl [$ret ['salesID']];
		$result ['qrCodeUrl'] = 'http://htm.zfdmkj.com/template_1/images/mobile/salesContact/'.$ret['salesID'].'_qr.png';
	}
}
echo PageUtil::setJsonReturn ( 1, $result );
exit ();