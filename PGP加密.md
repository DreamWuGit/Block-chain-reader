今天说说PGP加密

## 含义
PGP加密系统是采用公开密钥加密与传统密钥加密相结合的一种加密技术。它使用一对数学上相关的钥匙，其中一个（公钥）用来加密信息，另一个（私钥）用来解密信息。
PGP采用的传统加密技术部分所使用的密钥称为“会话密钥”（sek）。每次使用时，PGP都随机产生一个128位的IDEA会话密钥，用来加密报文。
公开密钥加密技术中的公钥和私钥则用来加密会话密钥，并通过它间接地保护报文内容。

PGP（Pretty Good Privacy），是一个基于RSA公匙加密体系的邮件加密软件。可以用它对你的邮件保密以防止非授权者阅读，
它还能对你的邮件加上数字签名从而使收信人可以确信邮件是你发来的。它让你可以安全地和你从未见过的人们通讯，事先并不需要任何保密的渠道用来传递密匙。
它采用了：审慎的密匙管理，一种RSA和传统加密的杂合算法，用于数字签名的邮件文摘算法，加密前压缩等，还有一个良好的人机工程设计。
它的功能强大，有很快的速度。而且它的源代码是免费的。

## 流程
PGP中的每个公钥和私钥都伴随着一个密钥证书。它一般包含以下内容：
密钥内容(用长达百位的大数字表示的密钥)
密钥类型(表示该密钥为公钥还是私钥)
密钥长度(密钥的长度，以二进制位表示)
密钥编号(用以唯一标识该密钥)
创建时间
用户标识 (密钥创建人的信息，如姓名、电子邮件等)
密钥指纹(为128位的数字，是密钥内容的提要表示密钥唯一的特征)
中介人签名(中介人的数字签名，声明该密钥及其所有者的真实性，包括中介人的密钥编号和标识信息)

PGP把公钥和私钥存放在密钥环(KEYR)文件中。PGP提供有效的算法查找用户需要的密钥。
PGP在多处需要用到口令，它主要起到保护私钥的作用。由于私钥太长且无规律，所以难以记忆。PGP把它用口令加密后存入密钥环，
这样用户可以用易记的口令间接使用私钥。
PGP的每个私钥都由一个相应的口令加密。PGP主要在3处需要用户输入口令：

需要解开收到的加密信息时，PGP需要用户输入口令，取出私钥解密信息
当用户需要为文件或信息签字时，用户输入口令，取出私钥加密
对磁盘上的文件进行传统加密时，需要用户输入口令
以上介绍了PGP的工作流程，下面将简介与PGP相关的加密、解密方法以及PGP的密钥管理机制。

## 加解密方法及密钥管理
PGP是一种供大众使用的加密软件。电子信息通过开放的网络传输，网络上的其他人都可以监听或者截取信息，来获得信息的内容，因而信息的安全问题就比较突出了。
保护信息不被第三者获得，这就需要加密技术。还有一个问题就是信息认证，如何让收信人确信信息没有被第三者篡改，这就需要数字签名技术。
RSA公匙体系的特点使它非常适合用来满足上述两个要求：保密性（Privacy)和认证性（Authentication）。
RSA（Rivest-Shamir-Adleman）算法是一种基于大数不可能质因数分解假设的公匙体系。简单地说就是找两个很大的质数，
一个公开即公钥，另一个不告诉任何人，即私钥。这两个密匙是互补的，就是说用公匙加密的密文可以用私匙解密，反过来也一样。

假设甲要寄信给乙，他们互相知道对方的公匙。甲就用乙的公匙加密信件寄出，乙收到后就可以用自己的私匙解密出甲的原文。由于没别人知道乙的私匙，所以即使是甲本人也无法解密那封信，这就解决了信件保密的问题。
另一方面由于每个人都知道乙的公匙，他们都可以给乙发信，那么乙就无法确信是不是甲的来信。这时候就需要用数字签名来认证。
在说明数字签名前先要解释一下什么是“信息摘要”(message digest)。信息摘要就是对信息用某种算法算出一个最能体现该信息的特征的数来，一旦信息有任何改变这个数都会变化，
那么这个数加上作者的名字（实际上在作者的密匙里）还有日期等等，就可以作为一个签名了。
PGP是用一个128位的二进制数作为“信息摘要”的，用来产生它的算法叫MD5(message digest 5)。 MD5是一种单向散列算法，它不像CRC校验码，
很难找到一份替代的信息与原件具有同样的MD5特征值。
回到数字签名上来，甲用自己的私匙将上述的128位的特征值加密，附加在信息后，再用乙的公匙将整个信息加密。这样这份密文被乙收到以后，
乙用自己的私匙将信息解密，得到甲的原文和签名，乙的PGP也从原文计算出一个128位的特征值来和用甲的公匙解密签名所得到的数比较，
如果符合就说明这份信息确实是甲寄来的。这样两个安全性要求都得到了满足

PGP还可以只签名而不加密，这适用于公开发表声明时，声明人为了证实自己的身份，可以用自己的私匙签名。这样就可以让收件人能确认发信人的身份，
也可以防止发信人抵赖自己的声明。
这一点在商业领域有很大的应用前途，它可以防止发信人抵赖和信件被中途篡改。
中本聪的发送邮件都采用的PGP加密, 所以至今才无人知道他的神秘身份
