---
layout: post
title: macOS에서 모든 커밋이 GitHub 인증을 받으려면?
excerpt: "How to do that all commits verify from GitHub on macOS"
categories: blog
tags: [GitHub, Verified, GPG, macOS]
image:
  feature: bg-studentsculpture.jpg
  credit: Stanford University, CA
comments: true
share: true
---

## 목차

- [개요](#개요)
- [GPG 생성](#gpg-생성)
- [GPG key GitHub 등록](#gpg-key-github-등록)
- [추가 설정](#추가-설정)
- [마무리](#마무리)
- [참고](#참고)

## 개요

보통 맥북에서 [Git](https://git-scm.com/)을 사용해서 [GitHub](https://github.com/) Repository에 push를 하면, GitHub의 __Verified__ 표시를 얻을 수 없습니다. 그런데 상황에 따라 GitHub의 branch 보호 전략에 __Verified__ 를 요구되기도 하는데요. 어쩌면 좋을까요?

![GitHub Verified commit](../../images/blog-2019-11-25/github-verified-commit.png)

위 그림의 빨간색 표시가 된 것처럼 방법이 없는게 아닙니다! 다만 설정을 조금 해줘야 합니다. 😅

### GPG 생성

우선 GitHub의 문서 중 [Generating a new GPG key](https://help.github.com/en/github/authenticating-to-github/generating-a-new-gpg-key)을 살펴봅시다.

문서에서는 [GPG command line tools](https://www.gnupg.org/download/) 다운 받아서 설치하라고 했지만 저는 `brew`를 이용하도록 하겠습니다. [[참고](https://dev.to/wes/how2-using-gpg-on-macos-without-gpgtools-428f)]

```sh
brew install gnupg pinentry-mac
```

설치가 완료되면 이제 생성해보도록 하죠.

```sh
gpg --full-generate-key
```

아래의 그림처럼 설정하는 것을 권장합니다.

|:------------------------------------------------------------------:|:------------------------------------------------------------------:|
| ![GPG generate 1](../../images/blog-2019-11-25/gpg-generate-1.png) | ![GPG generate 2](../../images/blog-2019-11-25/gpg-generate-2.png) |

- (1) RSA and RSA
- keysize: `4096`
- valid: `0` (key does not expire)
- confirm valid: `y`
- Real name: 본인 이름 또는 닉네임
- Email address: GitHub에서 사용중인 이메일
- confirm: `O` (Okay)

아래 그림에서는 일반적인 암호(Password)라고 생각하고 입력해주세요. 나중에 딱 한 번 입력하게 됩니다. (__1__)

|:------------------------------------------------------------------:|:----------------------------------------------------------:|
| ![GPG passphrase](../../images/blog-2019-11-25/gpg-passphrase.png) | ![GPG finish](../../images/blog-2019-11-25/gpg-finish.png) |

```sh
gpg --list-secret-keys --keyid-format LONG
```

위 명령어를 입력하면 아래와 같은 결과가 나오고, 빨간색 네모 부분(16자리 숫자 + 문자)를 복사해둡니다. (__2__)

![GPG key](../../images/blog-2019-11-25/gpg-keyid.png)

```sh
gpg --armor --export COPIED_GPG_KEYID
```

결과값을 모두 복사해둡니다. (__3__)

```sh
-----BEGIN PGP PUBLIC KEY BLOCK-----

HAVE TO COPY ALL
-----END PGP PUBLIC KEY BLOCK-----
```

### GPG key GitHub 등록

GitHub의 [SSH and GPG keys](https://github.com/settings/keys) 메뉴를 보면 아래와 비슷하게 나올겁니다. (저는 사용 중인 SSH keys와 GPG keys가 하나씩 있습니다.) New GPG key를 클릭하여 새로운 key를 등록하도록 합니다. [[참고](https://help.github.com/en/github/authenticating-to-github/adding-a-new-gpg-key-to-your-github-account)]

![GitHub settings keys](../../images/blog-2019-11-25/github-settings-keys-before.png)

![GitHub settings keys](../../images/blog-2019-11-25/github-settings-new-gpg-key.png)

위 그림의 빈 칸에 터미널에서 복사한 Public key(__3__)를 넣어주고 등록해주세요. 그러면, 아래와 같이 새롭게 추가된 GPG key를 확인 하실 수 있습니다.

![GitHub settings keys](../../images/blog-2019-11-25/github-settings-keys-after.png)

### 추가 설정

GitHub [문서](https://help.github.com/en/github/authenticating-to-github/telling-git-about-your-signing-key)에 따르면, 명령어 하나만 입력하면 끝나는 것 같지만 몇 가지 더 설정을 해줘야 합니다.

우선 아래와 같이 Git global 세팅을 해줍니다. (GPG key ID가 필요합니다. (__2__) `gpg --list-secret-keys --keyid-format LONG`)

```sh
git config --global user.name "YOUR_NAME"
git config --global user.email "YOUR_EMAIL"
git config --global core.precomposeunicode true
git config --global core.quotepath false
git config --global user.signingkey COPIED_GPG_KEYID
git config --global gpg.program gpg
git config --global commit.gpgsign true
```

여기서 `git commit`을 시도하면 문제가 발생하는데요. 해결 방법은 [여기](https://stackoverflow.com/questions/39494631/gpg-failed-to-sign-the-data-fatal-failed-to-write-commit-object-git-2-10-0)를 참고하여 아래와 같이 입력해주세요.

```sh
echo "pinentry-program /usr/local/bin/pinentry-mac" >> ~/.gnupg/gpg-agent.conf
killall gpg-agent
```

그 후에 다시 `git commit`을 시도하면 아래와 같이 Pinentry Mac 화면이 나타납니다.

위에서 암호처럼 입력했던 (__1__)을 입력하고, __Save in Keychain__ 을 클릭해 주세요. 로컬에 저장되어 다시는 이 창을 볼 일 없이 편하게 사용할 수 있습니다.

![Pinentry Mac](../../images/blog-2019-11-25/pinentry-mac.png)

### 마무리

이제 모든 설정이 다 끝났습니다! GitHub에서 push된 모든 commit이 Verified 된 것을 확인하실 수 있습니다.

### 참고

- [Managing commit signature verification](https://help.github.com/en/github/authenticating-to-github/managing-commit-signature-verification)
- [GitHub: Generating a new GPG key](https://help.github.com/en/github/authenticating-to-github/generating-a-new-gpg-key)
- [Using GPG on macOS without GPGTools](https://dev.to/wes/how2-using-gpg-on-macos-without-gpgtools-428f)
- [Adding a new GPG key to your GitHub account](https://help.github.com/en/github/authenticating-to-github/adding-a-new-gpg-key-to-your-github-account)
- [Telling Git about your signing key](https://help.github.com/en/github/authenticating-to-github/telling-git-about-your-signing-key)
- [gpg failed to sign the data fatal: failed to write commit object](https://stackoverflow.com/questions/39494631/gpg-failed-to-sign-the-data-fatal-failed-to-write-commit-object-git-2-10-0)
