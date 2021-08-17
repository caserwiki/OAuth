# Oauth

## 介绍

第三方登录授权的SDK。

## 安装

```
composer require zhanxin/oauth
```

## 说明

公共方法：
- `getAuthUrl()` 获取授权地址
- `getAccessToken($storeState = null, $state = null, $code = null)` 获取AccessToken（只返回access_token）
- `getAccessTokenResult()` 执行`getAccessToken`方法后，此方法获取原结果
- `getUserInfo(string $accessToken)` 获取用户信息
- `validateAccessToken(string $accessToken)` 验证token是否有效
- `refreshToken(string $refreshToken = null)` 刷新token 返回`bool`
- `getRefreshTokenResult()` 执行`refreshToken`方法后，此方法获取原结果

## 使用

微信/QQ:
```php
class WeiXin
{
    public function index()
    {
        $config = new \OAuth\WeiXin\Config();
        $config->setAppId('appid');
        $config->setState('state');
        $config->setRedirectUri('redirect_uri');

        $oauth = new \OAuth\WeiXin\OAuth($config);
        $url = $oauth->getAuthUrl();

        return $this->response()->redirect($url);
    }

    public function callback()
    {
        $params = $this->request()->getQueryParams();

        $config = new \OAuth\WeiXin\Config();
        $config->setAppId('appid');
        $config->setSecret('secret');

        $oauth = new \OAuth\WeiXin\OAuth($config);
        $accessToken = $oauth->getAccessToken('state', $params['state'], $params['code']);
        $refreshToken = $oauth->getAccessTokenResult()['refresh_token'];

        $userInfo = $oauth->getUserInfo($accessToken);
        var_dump($userInfo);

        if (!$oauth->validateAccessToken($accessToken)) echo 'access_token 验证失败！' . PHP_EOL;


        if (!$oauth->refreshToken($refreshToken)) echo 'access_token 续期失败！' . PHP_EOL;

    }
}
```

微博:
```php
class Weibo
{
    public function index()
    {
        $config = new \OAuth\Weibo\Config();
        $config->setClientId('clientid');
        $config->setState('state');
        $config->setRedirectUri('redirect_uri');

        $oauth = new \OAuth\Weibo\OAuth($config);
        $url = $oauth->getAuthUrl();

        return $this->response()->redirect($url);
    }

    public function callback()
    {
        $params = $this->request()->getQueryParams();

        $config = new \OAuth\Weibo\Config();
        $config->setClientId('clientid');
        $config->setClientSecret('secret');
        $config->setRedirectUri('redirect_uri');

        $oauth = new \OAuth\Weibo\OAuth($config);
        $accessToken = $oauth->getAccessToken('state', $params['state'], $params['code']);

        $userInfo = $oauth->getUserInfo($accessToken);
        var_dump($userInfo);

        if (!$oauth->validateAccessToken($accessToken)) echo 'access_token 验证失败！' . PHP_EOL;
    }
}
```

支付宝:
```php
class AliPay
{
    public function index()
    {
        $config = new \OAuth\AliPay\Config();
        $config->setState('state');
        $config->setAppId('appid');
        $config->setRedirectUri('redirect_uri');

        $oauth = new \OAuth\AliPay\OAuth($config);
        $url = $oauth->getAuthUrl();
        return $this->response()->redirect($url);
    }

    public function callback()
    {
        $params = $this->request()->getQueryParams();

        $config = new \OAuth\AliPay\Config();
        $config->setAppId('appid');
        $config->setAppPrivateKey('私钥');

        $oauth = new \OAuth\AliPay\OAuth($config);
        $accessToken = $oauth->getAccessToken('state', $params['state'], $params['auth_code']);
        $refreshToken = $oauth->getAccessTokenResult()['alipay_system_oauth_token_response']['refresh_token'];

        $userInfo = $oauth->getUserInfo($accessToken);
        var_dump($userInfo);

        if (!$oauth->validateAccessToken($accessToken)) echo 'access_token 验证失败！' . PHP_EOL;
        var_dump($oauth->getAccessTokenResult());

        if (!$oauth->refreshToken($refreshToken)) echo 'access_token 续期失败！' . PHP_EOL;
        var_dump($oauth->getRefreshTokenResult());
    }
}
```

GitHub:
```php
class Github
{
    public function index()
    {
        $config = new \OAuth\Github\Config();
        $config->setClientId('clientid');
        $config->setRedirectUri('redirect_uri');
        $config->setState('state');
        $oauth = new \OAuth\Github\OAuth($config);
        $this->response()->redirect($oauth->getAuthUrl());
    }

    public function callback()
    {
        $params = $this->request()->getQueryParams();
        $config = new \OAuth\Github\Config();
        $config->setClientId('clientid');
        $config->setClientSecret('secret');
        $config->setRedirectUri('redirect_uri');

        $oauth = new \OAuth\Github\OAuth($config);
        $accessToken = $oauth->getAccessToken('state', $params['state'], $params['code']);
        $userInfo = $oauth->getUserInfo($accessToken);
        var_dump($userInfo);

        if (!$oauth->validateAccessToken($accessToken)) echo 'access_token 验证失败！' . PHP_EOL;
    }
}
```

码云:
```php
class Gitee
{
    public function index()
    {
        $config = new \OAuth\Gitee\Config();
        $config->setState('state');
        $config->setClientId('clientid');
        $config->setRedirectUri('redirect_uri');
        $oauth = new \OAuth\Gitee\OAuth($config);
        $this->response()->redirect($oauth->getAuthUrl());
    }

    public function callback()
    {
        $params = $this->request()->getQueryParams();

        $config = new \OAuth\Gitee\Config();
        $config->setClientId('client_id');
        $config->setClientSecret('secret');
        $config->setRedirectUri('redirect_uri');

        $oauth = new \OAuth\Gitee\OAuth($config);
        $accessToken = $oauth->getAccessToken('state', $params['state'], $params['code']);
        $userInfo = $oauth->getUserInfo($accessToken);
        var_dump($userInfo);

        if (!$oauth->validateAccessToken($accessToken)) echo 'access_token 验证失败！' . PHP_EOL;
        var_dump($oauth->getAccessTokenResult());
    }
}
```