---
title: Intro to input methods on Linux
---

This is a simple recommendation for your IME on Arch Linux. For I seldom download something from repositories except the official one, I would just like to introduce IMEs that can be found in core, community or extra repos.

Before that, you should know that there're two frameworks for IME development.

## Frameworks

### IBus

IBus is short for Intelligent Input Bus. It's built-in for GNOME desktop. For more details, refer to https://en.wikipedia.org/wiki/Intelligent_Input_Bus.

### Fcitx

Fcitx has a nice compatibility with IMEs of Chinese and other Asian languages, but not built-in and may need extra configuration for Arch. For more details, go to https://en.wikipedia.org/wiki/Fcitx.

## Chinese IME

There're mainly three choices for Chinese IME.

### ibus-libpinyin

This is a substitute for the deprecated package `ibus-pinyin`. It's the only (perhaps) suitable IME on IBus. But it does not have a wide vocabulary, and I guess it does not have a cloud or machine learning mechanism. Almost half of input words can't be found, so you have to input them manually.

Not recommended.

### fcitx-googlepinyin

Google Pinyin has used cloud and it's surely much better than `ibus-libpinyin`. It's no doubt that Google has a strong ability of cloud computing and machine learning, but unfortunately it's **USERS** that decide whether an IME is easy to use or not.

That is, the more people are using an IME, the more comfortable you'll feel when using it. The amount of data determines quality of service. It's not like Chrome or Google Drive, it's just a rule for IMEs.

Though, it has a fast switching speed than any IME else. If you're a Google fan and you do not quite care about the popular words used by most people now, maybe you should first try it.

### fcitx-sogoupinyin

We always look down on domestic softwares nowadays, let alone versions for Linux. But it is a rare one that I think is the **BEST** IME for Chinese. Large vocabulary, fast cloud connection, selectable themes... In fact, the only drawback I could find for it is that it starts up a little slower than Google Pinyin.

Strongly recommended.

## Japanese IME

There are indeed Anthy and Mozc for Japanese IME, but since I'm half a Google fan and Mozc originated from Google Japanese Input, I'll just introduce Mozc.

### fcitx-mozc

There's no version for mozc on IBus, and this is the only version you can choose if you use `mozc` and do not import AUR repo. By the way, Anthy has both `ibus` and `fcitx` versions.

In a word, it's good. `fcitx-mozc` is just enough for your use. Even if you want recent popular words (like names of anime), it will probably get what you are thinking. It's worthy of Google's fame.

## Conclusion

Now I'm using three keyboards on Fcitx:

- Keyboard English
- Sogou Pinyin
- Mozc

And I'm quite satisfied with my IMEs now. If you want to have a try, first ensure that you've installed corresponding framework (ibus and/or fcitx), and then use

```shell
pacman -Syu ibus-***/fcitx-***
```

to install your favorite IME.
