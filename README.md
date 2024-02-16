# Pripara HK app modification

A guide to edit apk `jp.co.takaratomy_arts.pripara` to allow scanning others people progress qrcode or friend qrcode and use its as your own.

below guide will show how to disable the qrcode type verification and some example of changing the name of the character.

it is also possible to change other values but I didn't needed it for my use case so it will be excluded from this guide.

## Pre-requirement

- make sure java is install
- [apktool](https://github.com/iBotPeaches/Apktool) 2.9.1
- [Uber Apk Signer](https://github.com/patrickfav/uber-apk-signer) 1.3.0

Some Notes

1. all requirement of newer version should work but are not tested, if its not working please use the exact version as mention below
2. move all required files to root direction of this repo as all command will run assuming its `pwd` were root of this project

## Step 1 unpack the apk

- download `Pripara_2.0_Apkpure.apk` from the internet
- make sure the app id is `jp.co.takaratomy_arts.pripara` and version is `2.0`
- run command `./apktool_2.9.1.jar d ./Pripara_2.0_Apkpure.apk`

## Steps 2 disable condition checking

- modify file `./Pripara_2.0_Apkpure/smali/jp/co/takaratomy_arts/pripara/c/p.smali` in line 422

from

```smali
if-eqz v0, :cond_4
```

to

```smali
# if-eqz v0, :cond_4
```

## Step 3 modify the username (optional)

if you do not need to modify the name, you can skip this part and it will keep using the original friend/user qrcode name

check `./extra/name.txt` for value of each character

`name.txt` on the left is the value, right is the display character

data structure, each character were store in 2 int formula as (a*255+b)

name of max string length were 6, total of 12 byte required to edit

`'i' is the index of the name`

if the `value - 255 > 0 is true`, then set the first part `char[i][0] = 1` second part to `char[i][1] = value - 255`

example `326 z`, `326-255=71`, so `char[i][0] = 0x1`, `char[i][1] = 0x47`, 0x47 is the hex value of 71

example `201 A`, `201-255=-54`, so `char[i][0] = 0x0`, `char[i][1] = 0xC9`, 0xC9 is the hex value of 201

- edit file `./Pripara_2.0_Apkpure/smali/jp/co/takaratomy_arts/pripara/f/a.smali` goto line `3168` you will see

```smali
# virtual methods
.method public a()Ljava/lang/String;
    .locals 4
    # -----------------
    # insert code here
    # -----------------
    new-instance v1, Ljava/lang/StringBuilder;
```

below example will result in name `★`

`415 ★`, `415-255=160`, so `char[0][0] = 0x1`, `char[0][1] = 0xA0`

as other character we want it to be blank, so `char[1][0] = 0x0` `char[1][1] = 0x0` ... `char[5][0] = 0x0` `char[5][1] = 0x0`

the code to be insert as below

```smali
    # start modify name

    # char[0][0]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x69 # data position
    const/16 v3, 0x1 # value of char[0][0]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[0][1]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x6a # data position
    const/16 v3, 0xA0 # value of char[0][1]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[1][0]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x6b # data position
    const/16 v3, 0x0 # value of char[1][0]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[1][1]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x6c # data position
    const/16 v3, 0x0 # value of char[1][1]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[2][0]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x6d # data position
    const/16 v3, 0x0 # value of char[2][0]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[2][1]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x6e # data position
    const/16 v3, 0x0 # value of char[2][1]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[3][0]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x6f # data position
    const/16 v3, 0x0 # value of char[3][0]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[3][1]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x70 # data position
    const/16 v3, 0x0 # value of char[3][1]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[4][0]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x71 # data position
    const/16 v3, 0x0 # value of char[4][0]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[4][1]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x72 # data position
    const/16 v3, 0x0 # value of char[4][1]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[5][0]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x73 # data position
    const/16 v3, 0x0 # value of char[5][0]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[5][1]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x74 # data position
    const/16 v3, 0x0 # value of char[5][1]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # end
```

result

```smali
# virtual methods
.method public a()Ljava/lang/String;
    .locals 4
    # start modify name

    # char[0][0]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x69 # data position
    const/16 v3, 0x1 # value of char[0][0]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[0][1]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x6a # data position
    const/16 v3, 0xA0 # value of char[0][1]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[1][0]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x6b # data position
    const/16 v3, 0x0 # value of char[1][0]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[1][1]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x6c # data position
    const/16 v3, 0x0 # value of char[1][1]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[2][0]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x6d # data position
    const/16 v3, 0x0 # value of char[2][0]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[2][1]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x6e # data position
    const/16 v3, 0x0 # value of char[2][1]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[3][0]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x6f # data position
    const/16 v3, 0x0 # value of char[3][0]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[3][1]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x70 # data position
    const/16 v3, 0x0 # value of char[3][1]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[4][0]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x71 # data position
    const/16 v3, 0x0 # value of char[4][0]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[4][1]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x72 # data position
    const/16 v3, 0x0 # value of char[4][1]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[5][0]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x73 # data position
    const/16 v3, 0x0 # value of char[5][0]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # char[5][1]
    iget-object v0, p0, Ljp/co/takaratomy_arts/pripara/f/a;->d:Ljava/util/List;
    const/16 v2, 0x74 # data position
    const/16 v3, 0x0 # value of char[5][1]
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v3
    invoke-interface {v0, v2, v3}, Ljava/util/List;->set(ILjava/lang/Object;)Ljava/lang/Object;

    # end
    new-instance v1, Ljava/lang/StringBuilder;
```

## Step 4 re-build and sign the apk

- run command to rebuild the apk `./apktool_2.9.1.jar b ./Pripara_2.0_Apkpure`
- result apk will be in `./Pripara_2.0_Apkpure/dist/Pripara_2.0_Apkpure.apk`
- run command to sign the apk `java -jar ./uber-apk-signer-1.3.0.jar -apks ./Pripara_2.0_Apkpure/dist`
- result apk will be in `./Pripara_2.0_Apkpure/dist/Pripara_2.0_Apkpure-aligned-debugSigned.apk`
- install `Pripara_2.0_Apkpure-aligned-debugSigned.apk` on to a supported device
- Scan a friend qrcode or another player qrcode on your device, if you don't have one you can scan my qrcode on below

you can choose one of below for scanning, should have no different

### my user qrcode

![my](extra/my.png)

### my friend qrcode

![my](extra/friend.png)

### Some other notes

After playing few games, it should allow you to change your appearance. but name, voice and birthday are not changeable. those will required to edit the apk or modify the qr code

Make sure to backup your qrcode as after modify apk it does not have qrcode checking, if you scan other people qrcode you may actually changing your account to their account.

It is also possible to export your data to a non-cracked APK app, just click export after you scanned/imported other people qrcode, then use another device to install a 'non-modify version apk' and import it.

Due to the original app are very buggy, on android 12 or above it may required to manually set permission 'allow notification' before starting the app.

Also the 'Update Profile Image' on some device it will crash the app
