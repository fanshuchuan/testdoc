行云WebGame GDP接入说明
======================


简介
----
行云Webgame GDP目前支持用户自主创建游戏加载页面，支持部署到包括337在内的多个webgame游戏平台；Webgame支持选"服"功能，服列表会随着使用云平台开服的进度随时更新。

登录验证
--------
在加载游戏url时，行云会为其拼接上如下参数，以便开发者进行用户真实性验证。

=================   ===========================
参数名              描述
=================   ===========================
sig_app_id          提前分配好的app id
sig_api_key         提前分配好的的api key
sig_auth_key        加密过后的验证参数
sig_user            用户UID
sig_time            Unix时间戳
sig_flash_xml_url   flash xml地址
=================   ===========================

例如，行云在加载游戏页面的时候会同时加上上面所述参数::

    <iframe name="iframe_game_panel" scrolling="auto" frameborder="0" src="http://游戏路径/index.php?sig_auth_key={sig_auth_key}&sig_user={sig_user}&sig_app_id={sig_app_id}&sig_api_key={sig_api_key}&sig_time={sig_time}&sig_username={sig_username}&sig_flash_xml_url={sig_flash_xml}&connect_id={connect_id}" style="width:100%;"></iframe>


验证算法：``sig_auth_key = MD5.encrypt(sig_user + sig_app_id + sig_api_key + sig_time + secret)``

其中secret为双方约定的密钥，通过行云平台配置；游戏方在验证通过后，需要进一步对sig_time做有效时间的检查。

登录验证PHP代码示例::

    if(md5($_GET[‘sig_user’] . $_GET[‘sig_app_id’] . $_GET[‘sig_api_key’] . $_GET[‘sig_time’] . $secret) !==  $_GET[‘sig_auth_key’]) {
         error_log(“Bad Signature”);
         return null;
    } else if((time() - (int)$_GET[‘sig_time’]) > 60 * 5) {
         // request should be issued in 5 minutes for example
         error_log(“Request Timeout”);
         return null;
    }


JavaScript SDK说明
------------------

SDK文件地址
```````````
JS SDK gdp_jsproxy_client2.1.js依赖于JQuery和Swfobject，http及https的地址分别如下。

HTTP地址：

 * http://ajax.googleapis.com/ajax/libs/jquery/1.4.2/jquery.min.js
 * http://elex_p_img337-f.akamaihd.net/static/js/swfobject.js
 * http://elex_p_img337-f.akamaihd.net/static/js/common/gdp_jsproxy_client2.1.js

HTTPS地址：

 * https://ajax.googleapis.com/ajax/libs/jquery/1.4.2/jquery.min.js
 * https://elex-i.akamaihd.net/platform.elex-tech.us/static/js/swfobject.js
 * https://elex-i.akamaihd.net/platform.elex-tech.us/static/js/common/gdp_jsproxy_client2.1.js


接口调用说明
````````````

通过代理方法call_gdp_function(apiName, params, callback)调用Elex GDP提供的JS API。参数说明如下：

===========  =======================================
参数名       说明
===========  =======================================
apiName      待调用的API名称（必须），如showPayments
params       API参数（可选）, 为object类型
callback     JS回调函数（可选）
===========  =======================================


代码示例
````````
显示支付套餐页面::

    call_gdp_function("showPayments", {role_id: “玩家角色ID”});

邀请好友::

    call_gdp_function("showInviteFriends", null, function(ret) {
          // 返回结果为邀请好友的uid数组，如["10000912092", "1000012898"]
          console && console.log(ret);
    });

发送Feed::

    var feedParams = {
          title:'DDTank',
          body:'welcome the world of DDTank',
          title_link:'http://apps.facebook.com/ddtank-br/',
          img:'http://p.img.337.com/337/img/br/uploadfile/ddt_120_130.gif?2011050401'
    };
    call_gdp_function("postFeed", feedParams, function(ret) {
          // ret为true时，feed发送成功，否则发送失败
          ret && alert('Post feed OK!');
    });

.. note::
    目前showInviteFriends、postFeed接口仅限于Facebook平台接入；showPayments接口各平台接入均支持。
