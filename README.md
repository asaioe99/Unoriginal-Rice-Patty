

https://user-images.githubusercontent.com/43358273/127954308-5519436a-d980-420d-a6cd-d21e3cf463e3.mp4

# 偽本田

*"低品質のガレージドアがホンダのセキュリティよりもマシだなんて信じられる？"*

*"Unoriginal-Rice-Patty" は、私の個人的な ホンダやアキュラに対するリプレイ攻撃のタイトルです。*
*"Honda" という日本語を翻訳すると、 "Original Rice Patty"（訳注：訳は「オリジナルの稲田」であるとしている。当然誤訳であり、正しくは新田に対する「昔からある田」である）になります。可愛らしい事実ですが、この攻撃方法はその名前に反して全く可愛くもないし、筆者のオリジナルでもない。*

*本記事の内容は、あくまでも教育目的のみに限られます。 本レポジトリに書いてある方法を実際に実行しても、私には一切責任はありません。自分の行動には責任を持ちましょう。*

この攻撃方法は、恐らく全てのホンダ／アキュラ製の自動車に使用されるリモート／ワイヤレス無線エントリーに有効であると思われます。ホンダは、未だに巡回鍵（rolling code）システムの採用をして*おらず*、セキュリティに対する配慮の無い固定鍵によるシステム*のみ*を製造している。

## 要約:
A hacker can gain complete and unlimited access to locking, unlocking, controlling the windows, opening the trunk, and starting the engine of the target vehicle where the only way to prevent the attack is to either never use your fob or, after being compromised (which would be difficult to realize), resetting your fob at a dealership.

## 攻撃:
単純に、FOB より送信された信号をキャプチャーすることで、少なくともある種の車両の制御をする上では十分である。If the target locks their vehicle, all it takes is
receiving it and saving it for me to gain the ability to replay the same command and have the vehicle respond accordingly.

Recording the "unlock" command from the target and replaying (this works on most if not all of Honda's produced FOBs) will allow me to unlock the vehicle whenever I'd like to, and it doesn't stop there *at all*
On top of being able to start the vehicle's *ENGINE* *Whenever I wished* through recording the "remote start", it seems possible to actually (through Honda's "Smart Key" which uses FSK) demodulate any command, edit it, and retransmit in order to make the target vehicle do whatever you wish.

例:
Recording a "lock" command and flipping the following bits: 

```653-656, 667-668, 677-680, 683-684, 823-826, 837-838, 847-850, 853-854```

will tell the vehicle to unlock (These numbers include the preamble)

I discovered this while analyzing the codes with my published Python script, "DiffBits" which is based on Samy Kamkars Perl script of the same name.

Here is the comparison of two "unlock" keys (no preamble):
<img width="1799" alt="unlock" src="https://user-images.githubusercontent.com/43358273/127954845-8e1503fd-2be6-4378-a452-96cd5b6c98b0.png">

Here is the comparison of two "lock" keys (no preamble):
<img width="1850" alt="lock" src="https://user-images.githubusercontent.com/43358273/127954862-3c6cd120-b9fe-43a9-955b-97c22e5d81e7.png">

Here is the comparison of one unlock, and one lock key (no preamble):
<img width="1850" alt="both" src="https://user-images.githubusercontent.com/43358273/127954890-bcd52cff-3782-4c3c-b1f5-4d926b1b56a5.png">

The red text indicates a change in bits. Some can be written off as simple errors in processing the codes, however some are very clearly indicating
what action the vehicle must perform.

(Codes processed with [URH](https://github.com/jopohl/urh) and were provided by [REDACTED]. Thanks, [REDACTED]!)

## *CONFIRMED* Vehicles (despite the fact that this likely affects them all):

• 2009 Acura TSX

• 2016 Honda Accord V6 Touring Sedan

• 2017 Honda HR-V (CVE-2019-20626)

• 2018 Honda Civic Hatchback

• 2020 Honda Civic LX


## The Interesting Part
Honda seemingly ignored CVE-2019-20626 (cited above in the list of confirmed, affected vehicles). They continued to implement 0 security measures against
this very simple "replay/replay and edit" attack. This CVE interestingly only cites one vehicle and I only discovered this much later in my pursuit for
research. Honda will not respond to me, or seemingly anyone attempting to report this security MAJOR flaw.

## ホンダは如何にして修正すべきか
Honda must implement a "rolling code" system into their vehicles' list of security measures. Rolling code systems have been around since 1995 and
work very well against hackers. Honda has seemingly never implemented this security measure, leaving them very far behind in the race toward a secure
technological future. As far as I can tell, this isn't easily "patchable". Honda can begin to implement security measures in future vehicles, however
it doesn't seem likely that they will go back and fix this security issue in older models.

## エピローグ
I am very interested in community feedback! Feel free to contact me with more research, information, or questions!
