# wujun
<?php
namespace App\libraries\Email;

use App\Models\CustomerEmail;

header("Content-type: text/html; charset=utf-8");

/**
 * 接收邮件api
 * Class ReceiveEmail
 */
class ReceiveEmail
{
    private static $server = '';
    private static $username = '';
    private static $password = '';
    private static $marubox = '';
    private static $email = '';
    private static $_instance;//ReceiveEmail单例
    public static $mail_status;
    public static $mail_count = 0;
    public static $_savePath = './upload/mail/receive/';
    public static $webPath = "./upload/mail/receive/";
    public static $amz_account = array(
        'FX' => array(
            'name' => 'EFE_JP',
            'short' => 'EFE_JP',
            'username' => 'iefiel_jp@iefiel.net',
            'password' => 'IEFIEjapan22.',
        ),
        'FX_US' => array(
            'name' => 'FX_US',
            'short' => 'FX_US',
            'username' => 'support_us@feeshow.cn',
            'password' => 'fxUS020!',
        ),
//        'FX_UK' => array(
//            'name' => 'FX_UK',
//            'short' => 'FX_UK',
//            'username' => 'support_uk@feeshow.cn',
//            'password' => '52ukFX?',
//        ),
//        'FX_AU' => array(
//            'name' => 'FX_AU',
//            'short' => 'FX_AU',
//            'username' => 'support_au@feeshow.cn',
//            'password' => 'Xiuau68#',
//        ),
//
//        /*'YD' => array(
//            'name' => '云端通',
//            'short' => 'YD',
//            'username' => 'customerservice@iiniim.com',
//            'password' => 'Meiyoumima123',
//        ),*/
//        'YD_US' => array(
//            'name' => 'YD_US',
//            'short' => 'YD_US',
//            'username' => 'customservice01@iiniim.cn',
//            'password' => 'YDuscs09?',
//        ),
//        'YD_UK' => array(
//            'name' => 'YD_UK',
//            'short' => 'YD_UK',
//            'username' => 'customservice02@iiniim.cn',
//            'password' => 'CSukyd09?',
//        ),
//        'YD_AU' => array(
//            'name' => 'YD_AU',
//            'short' => 'YD_AU',
//            'username' => 'customservice03@iiniim.cn',
//            'password' => '0424ydAU!',
//        ),
//        'YD_IT' => array(
//            'name' => 'YD_IT',
//            'short' => 'YD_IT',
//            'username' => 'cs.it@iiniim.cn',
//            'password' => 'IMydes24?',
//        ),
//        /*'YD_ES' => array(
//            'name' => 'YD_ES',
//            'short' => 'YD_ES',
//            'username' => 'cs.es@iiniim.cn',
//            'password' => 'IMydes22?',
//        ),*/
//        /*'CYM' => array(
//            'name' => '超伊美',
//            'short' => 'CYM',
//            'username' => 'customerservice@freebily.com',
//            'password' => 'Meiyoumima123',
//        ),*/
//        'CYM_US' => array(
//            'name' => 'CYM_US',
//            'short' => 'CYM_US',
//            'username' => 'support001@freebily.cn',
//            'password' => 'Chaoym02?',
//        ),
//        'CYM_UK' => array(
//            'name' => 'CYM_UK',
//            'short' => 'CYM_UK',
//            'username' => 'support002@freebily.cn',
//            'password' => 'UKchaoym52!',
//        ),
//        /*'CT' => array(
//            'name' => '冲拓',
//            'short' => 'CT',
//            'username' => 'customerservice@chictry.com',
//            'password' => 'Meiyoumima123',
//        ),*/
//        'CT_US' => array(
//            'name' => 'CT_US',
//            'short' => 'CT_US',
//            'username' => 'cs001@chictry.cn',
//            'password' => 'Chongtry320#',
//        ),
//        'CT_UK' => array(
//            'name' => 'CT_UK',
//            'short' => 'CT_UK',
//            'username' => 'cs002@chictry.cn',
//            'password' => 'Trychicuk06!',
//        ),
//        /*'EFE' => array(
//            'name' => 'EFE',
//            'short' => 'EFE',
//            'username' => 'customerservice@iefiel.com',
//            'password' => 'Xh2140',
//        ),*/
//        'EFE_US' => array(
//            'name' => 'EFE_US',
//            'short' => 'EFE_US',
//            'username' => 'cs01@iefiel.net',
//            'password' => 'Uscstefe01!',
//        ),
//        'EFE_UK' => array(
//            'name' => 'EFE_UK',
//            'short' => 'EFE_UK',
//            'username' => 'cs02@iefiel.net',
//            'password' => 'CSUKefe02?',
//        ),
//        'EFE_AU' => array(
//            'name' => 'EFE_AU',
//            'short' => 'EFE_AU',
//            'username' => 'cs03@iefiel.net',
//            'password' => 'aucsEFE03!',
//        ),
//        /*'TB' => array(
//            'name' => '跳步',
//            'short' => 'TB',
//            'username' => 'customerservice@tbshowing.com',
//            'password' => 'Xh424518',
//        ),*/
//        'TB_US' => array(
//            'name' => 'TB_US',
//            'short' => 'TB_US',
//            'username' => 'cs.us@tiaobug.cn',
//            'password' => 'Meiguotb259?',
//        ),
//        'TB_UK' => array(
//            'name' => 'TB_UK',
//            'short' => 'TB_UK',
//            'username' => 'cs.eu@tiaobug.cn',
//            'password' => 'tbUKcs09!',
//        ),
//        'TB_AU' => array(
//            'name' => 'TB_AU',
//            'short' => 'TB_AU',
//            'username' => 'cs.au@tiaobug.cn',
//            'password' => 'tbuAUgo23!',
//        ),
//        /*'SF' => array(
//            'name' => '四方',
//            'short' => 'SF',
//            'username' => 'customerservice@yizyifstore.com',
//            'password' => 'Xh2140',
//        ),*/
//        'SF_US' => array(
//            'name' => 'SF_US',
//            'short' => 'SF_US',
//            'username' => 'cs001@yizyif.cn',
//            'password' => 'SFuscs01!',
//        ),
//        'SF_UK' => array(
//            'name' => 'SF_UK',
//            'short' => 'SF_UK',
//            'username' => 'cs002@yizyif.cn',
//            'password' => 'SFukcs02!',
//        ),
//        'SF_AU' => array(
//            'name' => 'SF_AU',
//            'short' => 'SF_AU',
//            'username' => 'cs003@yizyif.cn',
//            'password' => 'auSF03?!',
//        ),
//
//        /*'WK' => array(
//            'name' => '唯快',
//            'short' => 'WK',
//            'username' => 'customerservice@iixpin.com',
//            'password' => 'Xh424518',
//        ),*/
//        'WK_US' => array(
//            'name' => 'WK_US',
//            'short' => 'WK_US',
//            'username' => 'cs.us@iixpin.net',
//            'password' => 'Legowk05!',
//        ),
//        'WK_UK' => array(
//            'name' => 'WK_UK',
//            'short' => 'WK_UK',
//            'username' => 'cs.uk@iixpin.net',
//            'password' => 'CSukWK2!',
//        ),
//        'iefiel.com' => array(
//            'name' => 'iefiel.com',
//            'short' => 'iefiel.com',
//            'username' => 'support@iefiel.com',
//            'password' => 'Support1209',
//        ),
//        'KM_US' => array(
//            'name' => 'KM_US',
//            'short' => 'KM_US',
//            'username' => 'cs001@dpois.cn',
//            'password' => 'KMmeiguo023!',
//        ),
//        'KM_UK' => array(
//            'name' => 'KM_UK',
//            'short' => 'KM_UK',
//            'username' => 'cs002@dpois.cn',
//            'password' => 'Ukkmhaha66?',
//        ),
//        'MM_US' => array(
//            'name' => 'MM_US',
//            'short' => 'MM_US',
//            'username' => 'cs-us@msemis.cn',
//            'password' => 'Miaomg09?!',
//        ),
//        'MM_UK' => array(
//            'name' => 'MM_UK',
//            'short' => 'MM_UK',
//            'username' => 'cs-uk@msemis.cn',
//            'password' => 'Miumiu814?',
//        ),
//        'HQS_US' => array(
//            'name' => 'HQS_US',
//            'short' => 'HQS_US',
//            'username' => 'uscs@alvivishop.com',
//            'password' => 'Hqs$123!',
//        ),
//        'HQS_UK' => array(
//            'name' => 'HQS_UK',
//            'short' => 'HQS_UK',
//            'username' => 'ukcs@alvivishop.com',
//            'password' => 'Hqs#123!',
//        ),
//        'agoky_US' => array(
//            'name' => 'agoky_US',
//            'short' => 'agoky_US',
//            'username' => 'uscs@agokystore.com',
//            'password' => 'Xh424518',
//        ),
//        'agoky_UK' => array(
//            'name' => 'agoky_UK',
//            'short' => 'agoky_UK',
//            'username' => 'ukcs@agokystore.com',
//            'password' => 'Xh424518',
//        ),
//        'ZX_US' => array(
//            'name' => 'ZX_US',
//            'short' => 'ZX_US',
//            'username' => 'uscs@acsussstore.com',
//            'password' => 'Youmima123',
//        ),
//        'Ranran_US' => array(
//            'name' => 'Ranran_US',
//            'short' => 'Ranran_US',
//            'username' => 'uscs@ranrann.com',
//            'password' => 'Hahago23!',
//        ),
//        'Ranran_UK' => array(
//            'name' => 'Ranran_UK',
//            'short' => 'Ranran_UK',
//            'username' => 'ukcs@ranrann.com',
//            'password' => 'Hahago23!',
//        ),
//        'LDY_US' => array(
//            'name' => 'LDY_US',
//            'short' => 'LDY_US',
//            'username' => 'csus@inlzdz.com',
//            'password' => 'Lago105?',
//        ),
//        'LDY_UK' => array(
//            'name' => 'LDY_UK',
//            'short' => 'LDY_UK',
//            'username' => 'csuk@inlzdz.com',
//            'password' => 'Lago105?',
//        ),
//        'HZY_US' => array(
//            'name' => 'HZY_US',
//            'short' => 'HZY_US',
//            'username' => 'csus@inhzoy.com',
//            'password' => 'Miu051!',
//        ),
//        'HZY_UK' => array(
//            'name' => 'HZY_UK',
//            'short' => 'HZY_UK',
//            'username' => 'csuk@inhzoy.com',
//            'password' => 'Miu051!',
//        ),
//        'YOOJIA_US' => array(
//            'name' => 'YOOJIA_US',
//            'short' => 'YOOJIA_US',
//            'username' => 'csus@yoojiashop.com',
//            'password' => 'Lago105?',
//        ),
//        'YOOJIA_UK' => array(
//            'name' => 'YOOJIA_UK',
//            'short' => 'YOOJIA_UK',
//            'username' => 'csuk@yoojiashop.com',
//            'password' => 'Lago105?',
//        ),
//        'ED_US' => array(
//            'name' => 'ED_US',
//            'short' => 'ED_US',
//            'username' => 'csus@yeahdor.com',
//            'password' => 'DaDa105?',
//        ),
//        'YHS_US' => array(
//            'name' => 'YHS_US',
//            'short' => 'YHS_US',
//            'username' => 'csus@yonghsstore.com',
//            'password' => 'aKAR9!',
//        ),
//        'ED_UK' => array(
//            'name' => 'ED_UK',
//            'short' => 'ED_UK',
//            'username' => 'csuk@yeahdor.com',
//            'password' => 'DaDa105?',
//        ),
//        'WY_US' => array(
//            'name' => 'WY_US',
//            'short' => 'WY_US',
//            'username' => 'csus@winyingstore.com',
//            'password' => 'Loolu88~',
//        )
    );
    public static $amz_account_DE = array(
        /*'FX' => array(
            'name' => '飞秀',
            'short' => 'FX',
            'username' => 'customerservice@feeshowing.com',
            'password' => 'Meiyoumima123',
        ),*/
        'EFE_DE' => array(
            'name' => 'EFE_DE',
            'short' => 'EFE_DE',
            'username' => 'efecsde02@iefiel.net',
            'password' => 'Ief1234',
        ),
        'TB_DE' => array(
            'name' => 'TB_DE',
            'short' => 'TB_DE',
            'username' => 'ebuy2nd@tiaobug.cn',
            'password' => 'Xiebin88!',
        ),
        'YD_DE' => array(
            'name' => 'YD_DE',
            'short' => 'YD_DE',
            'username' => 'amazon_de_yd@iiniim.com',
            'password' => 'Ydhome1234',
        ),
        'SF_DE' => array(
            'name' => 'SF_DE',
            'short' => 'SF_DE',
            'username' => 'yizyif_service@yizyif.cn',
            'password' => 'Tantt88!',
        ),
        'FX_DE' => array(
            'name' => 'FX_DE',
            'short' => 'FX_DE',
            'username' => 'support_de@feeshowing.com',
            'password' => 'Feeshow1234',
        ),
        'CT_DE' => array(
            'name' => 'CT_DE',
            'short' => 'CT_DE',
            'username' => 'de_support@chictry.com',
            'password' => 'Chictry1234',
        ),
        'CYM_DE' => array(
            'name' => 'CYM_DE',
            'short' => 'CYM_DE',
            'username' => 'chaoyimeide@freebily.com',
            'password' => 'CYMdeutschland77',
        ),
        'WK_DE' => array(
            'name' => 'WK_DE',
            'short' => 'WK_DE',
            'username' => 'wk_de@iixpin.com',
            'password' => 'Iixpin1234',
        ),
        'KM_DE' => array(
            'name' => 'KM_DE',
            'short' => 'KM_DE',
            'username' => 'supportde@dpois.com',
            'password' => 'kiTTY0614#',
        ),
        'HQS_DE' => array(
            'name' => 'HQS_DE',
            'short' => 'HQS_DE',
            'username' => 'hqsde@alvivishop.com',
            'password' => 'Acfunny123',
        ),
        'FXD_DE' => array(
            'name' => 'FXD_DE',
            'short' => 'FXD_DE',
            'username' => 'desupport@agokystore.com',
            'password' => 'DEfxiong03!',
        )

    );
    public static $amz_account_FR = array(
        'EFE_FR' => array(
            'name' => 'EFE_FR',
            'short' => 'EFE_FR',
            'username' => 'iefiel_fr@iefiel.com',
            'password' => 'A1887415157xx',
        ),
        'TB_FR' => array(
            'name' => 'TB_FR',
            'short' => 'TB_FR',
            'username' => 'amazon_fr_tb@tbshowing.com',
            'password' => '19930329Wq',
        ),
        'YD_FR' => array(
            'name' => 'YD_FR',
            'short' => 'YD_FR',
            'username' => 'frserviceyd@iiniim.com',
            'password' => 'Ydtong02#',
        ),
        'SF_FR' => array(
            'name' => 'SF_FR',
            'short' => 'SF_FR',
            'username' => 'sffr@yizyif.cn',
            'password' => '19930329Wq',
        ),
        'FX_FR' => array(
            'name' => 'FX_FR',
            'short' => 'FX_FR',
            'username' => 'feeshowfr@feeshow.cn',
            'password' => 'Fexhow15?',
        ),
        'CT_FR' => array(
            'name' => 'CT_FR',
            'short' => 'CT_FR',
            'username' => 'ct_fr@chictry.com',
            'password' => '356Youmi!',
        ),
        'CYM_FR' => array(
            'name' => 'CYM_FR',
            'short' => 'CYM_FR',
            'username' => 'amazon_cym_fr@freebily.com',
            'password' => 'A1887415157xx',
        ),
        'WK_FR' => array(
            'name' => 'WK_FR',
            'short' => 'WK_FR',
            'username' => 'cs.fr@iixpin.net',
            'password' => 'Weikuai02!',
        ),
        'KM_FR' => array(
            'name' => 'KM_FR',
            'short' => 'KM_FR',
            'username' => 'supportfr@dpois.com',
            'password' => 'Nadia1110',
        ),
        'HQS_FR' => array(
            'name' => 'HQS_FR',
            'short' => 'HQS_FR',
            'username' => 'hqsfr@alvivishop.com',
            'password' => 'Hqs2333',
        ),
        'FXD_FR' => array(
            'name' => 'FXD_FR',
            'short' => 'FXD_FR',
            'username' => 'frsupport@agokystore.com',
            'password' => 'Agoky2018',
        )


    );
    public static $amz_account_ES = array(
        'EFE_ES' => array(
            'name' => 'EFE_ES',
            'short' => 'EFE_ES',
            'username' => 'service_es@iefiel.net',
            'password' => '56724Ff!',
        ),
        'TB_ES' => array(
            'name' => 'TB_ES',
            'short' => 'TB_ES',
            'username' => 'cs.es@tiaobug.cn',
            'password' => 'esTiaobu03!',
        ),
        'YD_ES' => array(
            'name' => 'YD_ES',
            'short' => 'YD_ES',
            'username' => 'cs.es@iiniim.cn',
            'password' => 'IMydes22?',
        ),
        'SF_ES' => array(
            'name' => 'SF_ES',
            'short' => 'SF_ES',
            'username' => 'support_es@yizyifstore.com',
            'password' => 'Xmj1234',
        ),
        'FX_ES' => array(
            'name' => 'FX_ES',
            'short' => 'FX_ES',
            'username' => 'support_es@feeshowing.com',
            'password' => 'HZYfx68!',
        ),
        'CT_ES' => array(
            'name' => 'CT_ES',
            'short' => 'CT_ES',
            'username' => 'ct_es@chictry.com',
            'password' => 'Youmi356!',
        ),
        'CYM_ES' => array(
            'name' => 'CYM_ES',
            'short' => 'CYM_ES',
            'username' => 'amazon_cym_es@freebily.com',
            'password' => 'Haiya89!',
        ),
        /*'WK_ES' => array(
            'name' => 'WK_ES',
            'short' => 'WK_ES',
            'username' => 'cs.fr@iixpin.net',
            'password' => 'Weikuai02!',
        ),*/
        'HQS_ES' => array(
            'name' => 'HQS_ES',
            'short' => 'HQS_ES',
            'username' => 'hqses@alvivishop.com',
            'password' => 'ESgo029!',
        ),
        'FXD_ES' => array(
            'name' => 'FXD_ES',
            'short' => 'FXD_ES',
            'username' => 'essupport@agokystore.com',
            'password' => 'ESgo030!',
        )
    );
    public static $amz_account_JP = array(
        'EFE_JP' => array(
            'name' => 'EFE_JP',
            'short' => 'EFE_JP',
            'username' => 'iefiel_jp@iefiel.net',
            'password' => 'IEFIEjapan22.',
        ),
        'TB_JP' => array(
            'name' => 'TB_JP',
            'short' => 'TB_JP',
            'username' => 'csjp@tbshowing.com',
            'password' => 'Jpantb65#',
        ),
        'YD_JP' => array(
            'name' => 'YD_JP',
            'short' => 'YD_JP',
            'username' => 'support_jp@iiniim.com',
            'password' => 'Aa123456789',
        ),
        'FX_JP' => array(
            'name' => 'FX_JP',
            'short' => 'FX_JP',
            'username' => 'customerservice_jp@feeshowing.com',
            'password' => 'HZYhzy888',
        ),
        'HQS_JP' => array(
            'name' => 'HQS_JP',
            'short' => 'HQS_JP',
            'username' => 'jqscs_jp@alvivishop.com',
            'password' => 'Hppy#02',
        ),
        'FXD_JP' => array(
            'name' => 'FXD_JP',
            'short' => 'FXD_JP',
            'username' => 'fxdcs_jp@agokystore.com',
            'password' => 'iNGlar!06',
        )
    );
    public static $cd_account_FR = array(
        'cd_iefiel' => array(
            'name' => 'IEFIEL_FR',
            'short' => 'IEFIEL_FR',
            'username' => 'fblousefe@iefiel.net',
            'password' => '0620Creamy@'
        ),
        'cd_yizyif' => array(
            'name' => 'YIZYIF_FR',
            'short' => 'YIZYIF_FR',
            'username' => 'cutebabe@yizyifstore.com',
            'password' => 'Shi0707fang~'
        ),
        'cd_iiniim' => array(
            'name' => 'IINIIM_FR',
            'short' => 'IINIIM_FR',
            //'username' => 'fancyydt@iiniim.com',//主邮箱
            'username' => 'cidics@iiniim.cn',//附属邮箱
            //'password' => 'Gala0201#!'
            'password' => 'Ly180918'
        ),
        'cd_cym' => array(
            'name' => 'CYM_FR',
            'short' => 'CYM_FR',
            'username' => 'misspretty@freebily.com',
            'password' => 'Bilygo32!'
        ),
        'cd_ct' => array(
            'name' => 'CT_FR',
            'short' => 'CT_FR',
            'username' => 'cloverit@chictry.com',
            'password' => 'LVcomes20#'
        ),
    );



    //覆盖__clone()方法，禁止克隆
    private function __clone()
    {
    }


    /**
     * @return mixed
     */
    public static function getInstance()
    {
        if (!isset(self::$_instance) || (self::$_instance === NULL)) {
            self::$_instance = new ReceiveEmail();
        }
        return self::$_instance;
    }

    /**
     * Open an IMAP stream to a mailbox
     * @param $username
     * @param $password
     * @param $email_address
     * @param string $server
     * @param string $mail_server
     * @param string $server_type
     * @param int $port
     * @param bool $ssl
     * @internal param string $mailbox
     */
    public function connect($username, $password, $email_address, $server = '', $mail_server = 'imap.exmail.qq.com', $server_type = 'imap', $port = 993, $ssl = false)
    {

        if (!$server) {
            if ($server_type == 'imap') {
                if ($port == '') $port = '143';
                $str_connect = '{' . $mail_server . ':' . $port . '/imap/ssl}';
            } else {
                if ($port == '') $port = '110';
                $str_connect = '{' . $mail_server . ':' . $port . '/pop3' . ($ssl ? "/ssl" : "") . '}';
            }
            self::$server = $str_connect;
        } else {
            self::$server = $server;
        }
        self::$username = $username;
        self::$password = $password;
        self::$email = $email_address;
        self::$marubox = @imap_open(self::$server, self::$username, self::$password);
        if (!self::$marubox) {
            echo "Error: " . imap_last_error() . "<br/>";
            return false;
        }
        return true;
    }

    /**
     * @param string $server
     * @return bool
     */
    public function reconnect($server = '')
    {
        if (!self::$marubox) return false;
        if (!$reopen = imap_reopen(self::$marubox, $server, CL_EXPUNGE)) {
            echo "Error: " . imap_last_error() . "<br/>";
            exit;
        }
        self::$server = $server;
        return $reopen;
    }

    /**
     * 取消/标记邮件
     * @param $uid
     * @param int $type
     * @return bool
     */
    public function mark_mail_flagged($uid, $type = 1)
    {
        $msgs = imap_search(self::$marubox, $type === 1 ? 'Flagged' : 'Unflagged', SE_UID);
        if (in_array($uid, $msgs)) {
            $msgno = imap_msgno(self::$marubox, $uid);
            imap_headerinfo(self::$marubox, $msgno);
            imap_body(self::$marubox, $msgno);
            return imap_setflag_full(self::$marubox, "$uid", $type === 1 ? "\\Flagged \\Seen" : "\\Seen \\Unflagged", ST_UID);
        } else {
            return false;
        }
    }

    /**
     * 标记邮件成已读
     * @param $uid
     * @return bool
     */
    public function mark_mail_read($uid)
    {
        return imap_setflag_full(self::$marubox, $uid, '\\Seen', ST_UID);
    }

    /**
     * 标记邮件成未读
     * @param $uid
     * @return bool
     */
    public function mark_mail_un_read($uid)
    {
        return imap_clearflag_full(self::$marubox, $uid, '\\Unseen', ST_UID);
    }

    /**
     * 获取邮件的头部
     * @param $mid
     * @return object
     */
    public function get_imap_header($mid)
    {
        return imap_headerinfo(self::$marubox, $mid);
    }

    /**
     * 格式化头部信息 $headerinfo get_imap_header 的返回值
     * @param $mail_header
     * @return array
     */
    public function get_header_info($mail_header)
    {
        $sender = isset($mail_header->from) && isset($mail_header->from[0]) ? $mail_header->from[0] : '';
        $sender_replyto = isset($mail_header->reply_to) && isset($mail_header->reply_to[0]) ? $mail_header->reply_to[0] : '';
        $header = array();
        if (isset($sender->mailbox) && strtolower($sender->mailbox) != 'mailer-daemon' && strtolower($sender->mailbox) != 'postmaster') {
            $header = array(
                'sender' => isset($sender->mailbox) && isset($sender->host) ? strtolower($sender->mailbox) . '@' . $sender->host : '',
                'personal' => isset($sender->personal) ? $this->_decode_mime_str($sender->personal) : '',
                'recver' => isset($sender_replyto->mailbox) && isset($sender_replyto->host) ? strtolower($sender_replyto->mailbox) . '@' . $sender_replyto->host : '',
                'subject' => isset($mail_header->subject) ? $this->_decode_mime_str($mail_header->subject) : '',
                'recvdate' => isset($mail_header->date) ? date('Y-m-d H:i:s', strtotime($mail_header->date)) : '',
                'tag' => isset($mail_header->date) ? date('Ymd', strtotime($mail_header->date)) : '',
                'flag' => isset($mail_header->Flagged) && $mail_header->Flagged === 'F' ? 1 : 0,
                'read' => isset($mail_header->Unseen) && $mail_header->Unseen === 'U' ? 1 : 0,
                'replied' => isset($mail_header->Answered) && $mail_header->Answered == 'A' ? 1 : 0,
//                'exmid' => isset($mail_header->message_id) && $mail_header->message_id ? $mail_header->message_id : null,
            );
        }
        return $header;
    }

    /**
     * 判断是否阅读了邮件 $headerinfo get_imap_header 的返回值
     * @param $headerinfo
     * @return bool
     */
    public function is_unread($headerinfo)
    {
        if (($headerinfo->Unseen == 'U')) return true;
        return false;
    }

    /**
     * 删除邮件
     * @param $mid
     * @return bool
     */
    public function delete_mail($mid)
    {
        if (!self::$marubox) return false;
        return imap_delete(self::$marubox, $mid, 0);
    }

    /**
     * 获取附件
     * @param $mid
     * @param $path
     * @return bool|string
     */
    public function get_attach($mid, $path)
    {
        if (!self::$marubox) return false;
        $struckture = imap_fetchstructure(self::$marubox, $mid);
        $ar = "";
        if ($struckture->parts) {
            foreach ($struckture->parts as $key => $value) {
                $enc = $struckture->parts[$key]->encoding;
                if ($struckture->parts[$key]->ifdparameters) {
                    $name = $this->_decode_mime_str($struckture->parts[$key]->parameters[1]->value);
                    $message = imap_fetchbody(self::$marubox, $mid, $key + 1);
                    switch ($enc) {
                        case 0:
                            $message = imap_8bit($message);
                            break;
                        case 1:
                            $message = imap_8bit($message);
                            break;
                        case 2:
                            $message = imap_binary($message);
                            break;
                        case 3:
                            $message = imap_base64($message);
                            break;
                        case 4:
                            $message = quoted_printable_decode($message);
                            break;
                        case 5:
                            break;
                    }
                    $filename = iconv("UTF-8", "GB2312//IGNORE", $name);
                    $fp = fopen($path . $filename, "w");
                    fwrite($fp, $message);
                    fclose($fp);
                    $ar = $ar . $name . ",";
                }
                // Support for embedded attachments starts here
                if (!empty($struckture->parts[$key]->parts)) {
                    foreach ($struckture->parts[$key]->parts as $keyb => $valueb) {
                        $enc = $struckture->parts[$key]->parts[$keyb]->encoding;
                        if ($struckture->parts[$key]->parts[$keyb]->ifdparameters) {
                            $name = $struckture->parts[$key]->parts[$keyb]->dparameters[0]->value;
                            $partnro = ($key + 1) . "." . ($keyb + 1);
                            $message = imap_fetchbody(self::$marubox, $mid, $partnro);
                            switch ($enc) {
                                case 0:
                                    $message = imap_8bit($message);
                                    break;
                                case 1:
                                    $message = imap_8bit($message);
                                    break;
                                case 2:
                                    $message = imap_binary($message);
                                    break;
                                case 3:
                                    $message = imap_base64($message);
                                    break;
                                case 4:
                                    $message = quoted_printable_decode($message);
                                    break;
                                case 5:
                                    break;
                            }
                            $fp = fopen($path . $name, "w"); //解决文件名包含中文报错：failed to open stream: Invalid argument 的问题
                            fwrite($fp, $message);
                            fclose($fp);
                            $ar = $ar . $name . ",";
                        }
                    }
                }
            }
        }
        $ar = substr($ar, 0, (strlen($ar) - 1));
        return $ar;
    }


    function GetAttach($mid, $path, $uid = 0, $time = '') // Get Atteced File from Mail
    {
        if (!self::$marubox)
            return false;

        $struckture = imap_fetchstructure(self::$marubox, $mid);
        $files = array();
        if ($struckture->parts) {
            foreach ($struckture->parts as $key => $value) {
                $enc = $struckture->parts[$key]->encoding;

                //取邮件附件
                if ($struckture->parts[$key]->ifdparameters) {
                    //命名附件,转码
                    $name = $this->decode_mime($struckture->parts[$key]->dparameters[0]->value);
                    // $name=$struckture->parts[$key]->dparameters[0]->value;
                    $filename = iconv("gbk", 'utf-8', $name);
                    $extend = explode(".", $name);
                    $file['extension'] = $extend[count($extend) - 1];
                    $file['pathname'] = $path . $filename;
                    $file['title'] = !empty($name) ? htmlspecialchars($filename) : str_replace('.' . $file['extension'], '', $filename);
                    $file['size'] = $struckture->parts[$key]->dparameters[1]->value;
//                  $file['tmpname']   = $struckture->parts[$key]->dparameters[0]->value;
                    if (@$struckture->parts[$key]->disposition == "ATTACHMENT") {
                        $file['type'] = 1;
                    } else {
                        $file['type'] = 0;
                    }
                    $files[] = $file;

                    $message = imap_fetchbody(self::$marubox, $mid, $key + 1);
                    if ($enc == 0)
                        $message = imap_8bit($message);
                    if ($enc == 1)
                        $message = imap_8bit($message);
                    if ($enc == 2)
                        $message = imap_binary($message);
                    if ($enc == 3)//图片
                        $message = imap_base64($message);
                    if ($enc == 4)
                        $message = quoted_printable_decode($message);
                    if ($enc == 5)
                        $message = $message;
                    if (!in_array($file['extension'], array('exe', 'EXE'))) { //array('pdf','doc','docx','jpg','png','gif','jpeg')
                        //$fp=fopen($path.$name,"w");
                        $f = str_replace(':', '', str_replace(' ', '', $time));
                        $dir = $path . $uid . '/';
                        @mkdir($dir, 0777);
                        $dir2 = $dir . $f . '/';
                        @mkdir($dir2, 0777);
                        $fp = fopen($dir2 . $name, "w");
                        fwrite($fp, $message);
                        fclose($fp);
                    }


                }
//                 // 处理内容中包含图片的部分
//                 if($struckture->parts[$key]->parts)
//                 {
//                     foreach($struckture->parts[$key]->parts as $keyb => $valueb)
//                     {
//                         $enc=$struckture->parts[$key]->parts[$keyb]->encoding;
//                         if($struckture->parts[$key]->parts[$keyb]->ifdparameters)
//                         {
//                             //命名图片
//                             $name=$this->decode_mime($struckture->parts[$key]->parts[$keyb]->dparameters[0]->value);
//                             $extend =explode("." , $name);
//                             $file['extension'] = $extend[count($extend)-1];
//                             $file['pathname']  = $this->setPathName($key, $file['extension']);
//                             $file['title']     = !empty($name) ? htmlspecialchars($name) : str_replace('.' . $file['extension'], '', $name);
//                             $file['size']      = $struckture->parts[$key]->parts[$keyb]->dparameters[1]->value;
// //                          $file['tmpname']   = $struckture->parts[$key]->dparameters[0]->value;
//                             $file['type']      = 0;
//                             $files[] = $file;

//                             $partnro = ($key+1).".".($keyb+1);

//                             $message = imap_fetchbody($this->marubox,$mid,$partnro);
//                             if ($enc == 0)
//                                    $message = imap_8bit($message);
//                             if ($enc == 1)
//                                    $message = imap_8bit ($message);
//                             if ($enc == 2)
//                                    $message = imap_binary ($message);
//                             if ($enc == 3)
//                                    $message = imap_base64 ($message);
//                             if ($enc == 4)
//                                    $message = quoted_printable_decode($message);
//                             if ($enc == 5)
//                                    $message = $message;
//                             if (in_array($file['extension'], array('pdf','doc','docx'))) {
//                                  $fp=fopen($path.$file['pathname'],"w");
//                                 fwrite($fp,$message);
//                                 fclose($fp);
//                              }

//                         }
//                     }
//                 }
            }
        }
        //move mail to taskMailBox
        //$this->move_mails($mid, $this->marubox);

        return $files;
    }

    /**
     * 读取邮件主体
     * @param $mid
     * @return bool|string
     */
    public function get_body($mid)
    {
        if (!self::$marubox) return false;
        $body = $this->_get_part(self::$marubox, $mid, "TEXT/HTML");
        if ($body == "")
            $body = $this->_get_part(self::$marubox, $mid, "TEXT/PLAIN");
        if ($body == "") {
            return "";
        }
        return $body;
    }

    function get_body8($mid, &$path, $imageList) // Get Message Body
    {
        if (!self::$marubox) return false;

        $body = $this->_get_part(self::$marubox, $mid, "TEXT/HTML");
        if ($body == "")
            $body = $this->_get_part(self::$marubox, $mid, "TEXT/PLAIN");
        if ($body == "") {
            return "";
        }
        //处理图片
        $body = $this->embed_images($body, $path, $imageList);
        return $body;
    }

    /*
             * @desc处理图片
             *
             */

    function embed_images(&$body, &$path, $imageList)
    {
        // get all img tags
        preg_match_all('/<img.*?>/', $body, $matches);
        if (!isset($matches[0])) return;

        foreach ($matches[0] as $img) {//var_dump($img);
            // replace image web path with local path
            preg_match('/src="(.*?)"/', $img, $m);
            if (!isset($m[1])) continue;
            $arr = parse_url($m[1]);
            if (!isset($arr['scheme']) || !isset($arr['path'])) continue;

//          if (!isset($arr['host']) || !isset($arr['path']))continue;
            if ($arr['scheme'] != "http") {
                $filename = explode("@", $arr['path']);
                $body = str_replace($img, '<img alt="" src="' . $path . $imageList[$filename[0]] . '" style="border: none;" />', $body);
            }
        }
        return $body;
    }

    /**
     * @param $structure
     * @return string
     */
    private function _get_mime_type(&$structure)
    {
        $primary_mime_type = array("TEXT", "MULTIPART", "MESSAGE", "APPLICATION", "AUDIO", "IMAGE", "VIDEO", "OTHER");

        if ($structure->subtype) {
            return $primary_mime_type[(int)$structure->type] . '/' . $structure->subtype;
        }
        return "TEXT/PLAIN";
    }

    /**
     * @param $stream
     * @param $msg_number
     * @param $mime_type
     * @param bool $structure
     * @param bool $part_number
     * @return bool|string
     */
    private function _get_part($stream, $msg_number, $mime_type, $structure = false, $part_number = false)
    {
        if (!$structure) $structure = imap_fetchstructure($stream, $msg_number);
        if ($structure) {
            if ($mime_type == $this->_get_mime_type($structure)) {
                if (!$part_number) {
                    $part_number = "1";
                }
                $text = imap_fetchbody($stream, $msg_number, $part_number);
                //file_put_contents('D:/project/www/b/'.$msg_number.'.txt', $text);
                if ($structure->encoding == 3) {
                    return iconv($structure->parameters[0]->value, 'UTF-8', imap_base64($text));
                } else if ($structure->encoding == 4) {
                    if ($structure->parameters[0]->value !== 'UTF-8') {
                        return iconv($structure->parameters[0]->value, 'UTF-8', imap_qprint($text));
                    } else {
                        return imap_qprint($text);
                    }
                } else {
                    $tmp_value = trim($structure->parameters[0]->value);
                    if (isset($structure->parameters[0]->value) && !empty($tmp_value)) {
                        return iconv($structure->parameters[0]->value, 'UTF-8', $text);
                    } else {
                        return $text;
                    }
                }
            }
            if ($structure->type == 1) /* multipart */ {
                foreach ($structure->parts as $index=>$sub_structure){
              /*  while (list($index, $sub_structure) = ($structure->parts)) {*/
                    $prefix = false;
                    if ($part_number) {
                        $prefix = $part_number . '.';
                    }
                    $data = $this->_get_part($stream, $msg_number, $mime_type, $sub_structure, $prefix . ($index + 1));
                    if ($data) {
                        return $data;
                    }
                }
            }
        }
        return false;
    }

    /**
     * 关闭 IMAP 流
     */
    public function close_mailbox()
    {
        if (!self::$marubox) return false;
        return imap_close(self::$marubox, CL_EXPUNGE);
    }

    /**
     * @param $string
     * @param string $charset
     * @return string
     */
    private function _decode_mime_str($string, $charset = "UTF-8")
    {
        $newString = '';
        $elements = imap_mime_header_decode($string); //imap_mime_header_decode — Decode MIME header elements
        for ($i = 0; $i < count($elements); $i++) {
            if ($elements[$i]->charset == 'default') $elements[$i]->charset = 'iso-8859-1';
            $newString .= iconv($elements[$i]->charset, $charset, $elements[$i]->text);
        }
        return $newString;
    }

    /**
     * Read the list of mailboxes
     * @param string $pattern
     * @return array|bool|string
     */
    public function get_mailboxes($pattern = '*')
    {
        if (!self::$marubox) return false;
        if (!$list = imap_list(self::$marubox, self::$server, $pattern)) {
            echo 'Error: Read the list of mailboxes';
            exit;
        }
        $arr = array();
        foreach ($list as $key => $value) {

            $folder = str_replace(array(self::$server, "其他文件夹", "/"), "", mb_convert_encoding($value, "UTF8", "UTF7-IMAP"));  //imap_mutf7_to_utf8($value)
            if (in_array($folder, array('Drafts', 'Deleted Messages', 'Junk'))) continue;
            $arr[$folder] = $value;
        }
        return $arr;
    }

    /**
     * 获取邮件详情
     * @param int $uid
     * @return array
     */
    public function get_mail_detail($uid = 0)
    {
        $mid = $this->get_msgno($uid);
        $header = $this->get_imap_header($mid);
        $mail = $this->get_header_info($header);
        //处理邮件附件
        @$this->GetAttach($mid, self::$_savePath, $uid, $mail['recvdate']); // 获取邮件附件
        $mail['content'] = $this->get_body($mid);
        $mail['content'] = $this->removeEmoji($mail['content']);
        $mail['mid'] = $uid;
        preg_match('/[0-9]{3}(-([0-9]{7})){2}/i', $mail['content'], $match);
        $mail['order_id'] = $match== null?'':$match['0'];
        return $mail;
    }

    public function get_mail_attach($uid = 0)
    {
        $mid = $this->get_msgno($uid);
        $header = $this->get_imap_header($mid);
        $mail = $this->get_header_info($header);
        //处理邮件附件
        $this->GetAttach($mid, self::$_savePath, $uid, $mail['recvdate']); // 获取邮件附件
        $mail['content'] = $this->get_body($mid);
        //$mail['content'] = $this->get_body($mid,self::$_savePath,$setsqlarr);
        $mail['content'] = $this->removeEmoji($mail['content']);
        $mail['mid'] = $uid;
        preg_match('/[0-9]{3}(-([0-9]{7})){2}/i', $mail['content'], $match);
        $mail['order_id'] = $match[0];
        return $mail;
    }

    /**
     * 移除表情
     * @param $text
     * @return mixed
     */
    public function removeEmoji($text)
    {
        // Match Emoticons
        $regexEmoticons = '/[\x{1F600}-\x{1F64F}]/u';
        $clean_text = preg_replace($regexEmoticons, '', $text);
        // Match Miscellaneous Symbols and Pictographs
        $regexSymbols = '/[\x{1F300}-\x{1F5FF}]/u';
        $clean_text = preg_replace($regexSymbols, '', $clean_text);
        // Match Transport And Map Symbols
        $regexTransport = '/[\x{1F680}-\x{1F6FF}]/u';
        $clean_text = preg_replace($regexTransport, '', $clean_text);
        // Match Miscellaneous Symbols
        $regexMisc = '/[\x{2600}-\x{26FF}]/u';
        $clean_text = preg_replace($regexMisc, '', $clean_text);
        // Match Dingbats
        $regexDingbats = '/[\x{2700}-\x{27BF}]/u';
        $clean_text = preg_replace($regexDingbats, '', $clean_text);

        $regexEm = '/[\x{10000}-\x{10FFFF}]/u';
        $clean_text = preg_replace($regexEm, '', $clean_text);
        return $clean_text;
    }

    /**
     * Gets the message sequence number for the given UID
     * @param int $uid
     * @return bool|int
     */
    public function get_msgno($uid = 0)
    {
        if (!self::$marubox) return false;
        if (!$msgno = imap_msgno(self::$marubox, intval($uid))) {
            echo 'Error:get msgno';
            exit;
        }
        return $msgno;
    }

    /**
     * @param string $type
     * @return array|bool
     */
    public function get_messages_uids($type = '')
    {
        if (!self::$marubox) return false;
        if ($type) {
            $search = imap_search(self::$marubox, "$type", SE_UID);
            if ($search) $sort = $search;
            else $sort = array();
        } else {
            if (!$sort = imap_sort(self::$marubox, SORTDATE, 1, SE_UID)) {
                $sort = array();
            }
        }
        return $sort;
    }

    /**
     * 回复邮件
     * @param string $to
     * @param string $subject
     * @param string $message
     * @param string $uid
     * @param string $tpl
     * @return bool
     */
    public function mail_reply($to = '', $subject = '', $message = '', $uid = '', $tpl = '', $image = '')
    {
        if (!self::$marubox) return false;
        include_once "application/libraries/PHPMailer/class.phpmailer.php";
        try {
            $mail = new PHPMailer(); // the true param means it will throw exceptions on errors, which we need to catch
            $mail->IsSMTP(); // telling the class to use SMTP
            $mail->Host = "smtp.exmail.qq.com"; // SMTP server
            $mail->SMTPAuth = true;                  // enable SMTP authentication
            $mail->Port = 25;
            $mail->Username = self::$username; // SMTP account username
            $mail->Password = self::$password;
            $mail->AddReplyTo($to, $to);
            $mail->SetFrom(self::$email);
            $mail->AddAddress($to);
            $mail->Subject = $subject;
            $mail->CharSet = "utf-8";
            if ($image) {
                if (is_array($image)) { //多附件
                    foreach ($image as $img) {
                        $mail->AddAttachment($img);      // attachment这里是添加的附件
                    }
                } else {
                    $mail->AddAttachment($image);      // attachment这里是添加的附件
                }

            }
//        $mail->AltBody = $tpl; // optional, comment out and test
            $mail->MsgHTML($message);
            if (!$mail->Send()) {
                echo "Mailer Error: " . $mail->ErrorInfo;
                return false;
            } else {
                $msgno = $this->get_msgno(intval($uid));
                return imap_setflag_full(self::$marubox, $msgno, '\\Answered');
            }
        } catch (phpmailerException $e) {
            echo "Mailer Error: " . $e->errorMessage();
            return false;
        }
    }

    /**
     * 获取邮箱基本信息
     * @return bool|object
     */
    public function mailbox_info()
    {
        if (!self::$marubox) return false;
        if (!$mailbox_info = imap_mailboxmsginfo(self::$marubox)) {
            echo "Error: get mailbox info" . PHP_EOL;
            exit;
        }
        return $mailbox_info;
    }

    /**
     * 对象销毁前关闭邮箱
     */
    public function __destruct()
    {
        $this->close_mailbox();
    }

    /**
     * 标记邮件已回复
     * @param $uid
     * @return bool
     */
    public function mail_reply_no_email($uid)
    {
        return imap_setflag_full(self::$marubox, $uid, '\\Answered', ST_UID);
    }

    /**
     * @description cd回复邮件
     * @author xiaosh
     * @20180823
     */
    public function cd_mail_reply($to = '', $subject = '', $message = '', $uid = '', $tpl = '')
    {
        if (!self::$marubox) return false;
        include_once "application/libraries/PHPMailer/class.phpmailer.php";
        try {
            $mail = new PHPMailer(); // the true param means it will throw exceptions on errors, which we need to catch
            $mail->IsSMTP(); // telling the class to use SMTP
            $mail->Host = "smtp.exmail.qq.com"; // SMTP server
            $mail->SMTPAuth = true;                  // enable SMTP authentication
            $mail->Port = 25;
            $mail->Username = self::$username; // SMTP account username
            $mail->Password = self::$password;
            $mail->AddReplyTo($to, $to);
            $mail->SetFrom(self::$email);
            $mail->AddAddress($to);
            $mail->Subject = $subject;
//        $mail->AltBody = $tpl; // optional, comment out and test
            $mail->MsgHTML($message);
            if (!$mail->Send()) {
                echo "Mailer Error: " . $mail->ErrorInfo;
                return false;
            } else {
                return true;
            }
        } catch (phpmailerException $e) {
            echo "Mailer Error: " . $e->errorMessage();
            return false;
        }
    }

    public function imap_num_msg_total()
    {
        if (!self::$marubox) return false;
        if (!$msgnum = @imap_num_msg(self::$marubox)) {
            echo 'Error:get msgnum_total';
            exit;
        }
        return $msgnum;
    }

    public function get_amazon_account($type)
    {
        $temp = CustomerEmail::query()->get();
        /*if ($type) {
            $temp = self::$CI->db->query("select * from `customer_email` where platform='Amazon' and site=\"$type\" ")->result_array();
        } else {
            $temp = self::$CI->db->query("select * from `customer_email` where platform='Amazon' and site='US' ")->result_array();
        }*/
        $str = '';
        foreach ($temp as $list) {
            $str .= ($str ? ";" : "") . $list['account'] . ":" . $list['short_name'] . ":" . $list['email'] . ":" . $list['password'];
        }

        return $this->account_format($str);
    }

    public function get_cd_account($accountName)
    {
        /*if ($accountName) {
            $temp = self::$CI->db->query("select * from `customer_email` where platform='CD' ")->result_array();
        }*/
        $str = '';
       /* foreach ($temp as $list) {
            $str .= ($str ? ";" : "") . $list['account'] . ":" . $list['short_name'] . ":" . $list['email'] . ":" . $list['password'];
        }*/
        $cdAccount = $this->account_format($str);
        return $cdAccount[$accountName];
    }

    public function account_format($temp)
    {
        $amazonAccount = array();
        if ($temp) {
            $accountArray = explode(';', $temp);
            foreach ($accountArray as $list) {
                if ($list) {
                    $tempList = explode(':', $list);
                    $amazonAccount[$tempList[0]] = array(
                        'name' => preg_match('/^(cd_)/', $tempList[0]) ? $tempList[1] : $tempList[0],
                        'short' => $tempList[1],
                        'username' => $tempList[2],
                        'password' => $tempList[3]
                    );
                }
            }
        }
        return $amazonAccount;
    }


    /*
         * decode_mime()转换邮件标题的字符编码,处理乱码
         */
    function decode_mime($str)
    {
        $str = imap_mime_header_decode($str);
        return $str[0]->text;
        //echo "str";print_r($str);
        if ($str[0]->charset != "default") {//echo "==".$str[0]->text;
            return iconv($str[0]->charset, 'utf8', $str[0]->text);
        } else {
            return $str[0]->text;
        }
    }




}

