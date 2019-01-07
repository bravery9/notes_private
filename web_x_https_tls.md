>[HTTPS连接最初的若干毫秒 2009经典译文](https://www.infoq.cn/article/HTTPS-Connection-Jeff-Moser)

>[TLS handshake概述 - IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm)

>[Transport Layer Security - Wikipedia](https://en.wikipedia.org/wiki/Transport_Layer_Security)


### 加密

* 对称加密(Symmetrical Encryption)
  * 1个密钥
    * 通信双方的加密与解密过程 都使用这同一个密钥
  * 对称加密的算法:`Blowfish，AES，RC4，DES，RC5, RC6` 最广泛使用的对称加密算法是AES-128，AES-192和AES-256
  * 对称加密的缺点:主要缺点是所涉及的各方必须在解密数据之前交换用于加密数据的密钥
  * 对称加密的优点:速度快


* 非对称加密(Asymmetrical Encryption)
  * 2个密钥
    * 公钥 公开的 主要用来加密消息
      * 使用公钥加密的消息只能使用私钥解密
    * 私钥 保密的 主要用来解密消息
      * 使用私钥加密的消息也可以使用公钥解密
  * 非对称加密的算法:`EIGamal，RSA，DSA，PKCS，Elliptic curve techniques(椭圆曲线)`
  * 非对称加密的优点:现代的算法 消息传输过程的安全性高 所以非对称加密在互联网上应用广泛
  * 对称加密的缺点:速度慢 非对称加密比对称加密花费的时间相对更多

### 非对称加密的安全性的原理

非对称加密算法 - RSA
RSA算法安全性的核心是:基于n的两个质数分解难题. 即对极大整数n做因数分解的难度等同于RSA的安全性.(n=p×q ,n为极大整数,且p和q都为质数 如13和61)

### TLS handshake

* TLS握手对象
  * TLS client(浏览器)
  * TLS server(Web Server)

* TLS握手过程
  * 1.确认当前TLS server(Web Server)使用的TLS协议的版本
  * 2.选择加密算法(Select cryptographic algorithms)
  * 3.二者交换数字证书并验证数字证书 从而验证了彼此的身份(Authenticate each other by exchanging and validating digital certificates.)
  * 4.二者使用使用非对称加密技术生成共享的密钥 用于对称加密(Use asymmetric encryption techniques to generate a shared secret key.)

* TLS握手结果
  * 二者有了一个密钥(对称加密密钥,能够用于与对方通信时对messages进行加密). 
  * 即 TLS握手使TLS client(浏览器)能够与TLS server(Web Server)建立二者间交流数据的密钥(对称加密密钥).
