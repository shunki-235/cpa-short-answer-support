---
layout: page
title: CPA短答トレーナー TestFlightプライバシーポリシー
permalink: /privacy/
---

最終更新日: 2026-07-23

このプライバシーポリシーは、CPA短答トレーナーのTestFlight betaに適用するための草案です。本アプリは公認会計士試験・短答式試験の学習支援を目的とした非公式アプリであり、試験実施機関または関係団体が提供、承認、監修する公式アプリではありません。
2026-07-23時点では、公開ページの同期、backendとGoogle Formsの保持期間運用、backendログのIPアドレス取扱確認、安全なアカウント削除依頼の受付手順が完了していません。これらを確認するまで、この草案を配布用の最終ポリシーとして扱いません。

## 収集する情報

初回TestFlightでは、以下の情報を扱います。
購入UIを含まない初回buildでは匿名Firebase認証だけを使い、Sign in with Appleを開始しません。
後続の実課金検証buildでApple認証を有効にした場合だけ、表の「実課金検証build」と記した情報を追加で扱います。

| 情報 | 取扱場所 | 用途 |
|---|---|---|
| Firebase UID | Firebase、backend | 匿名ユーザーまたはApple連携済みユーザーの識別、AI Tutorの認証、rate limit、濫用防止。`TYPO_REPORT_API_URL` 有効buildでは誤植報告の送信者識別にも使う |
| Apple Accountの認証識別子 | Apple、Firebase | 実課金検証buildで、購入前の本人確認と再インストール後のFirebase UID、購入権利の復旧に使う |
| IPアドレス、user agent | Firebase Authentication。IPアドレスはbackendのin-memory IP rate limiterでも処理する | 認証要求の処理、不正利用防止、セキュリティ保護。backendのIP rate limiterはIPアドレスをDBへ保存しないが、FastifyとCloud Loggingのrequest logに含まれる情報は未確認 |
| AI Tutorの利用回数と利用プラン | backendのCloud SQL | Firebase UIDに紐づくbackend内ユーザーID、action、plan、UTC日付、request count、更新日時を使い、ユーザー・プラン別の日次rate limitを適用する |
| AI Tutorへの質問文 | backend、LLM provider API | 回答生成、品質確認、問い合わせ対応 |
| AI Tutorの回答、引用情報、参照した問題ID、content version、監修状態、参照元ID | backend | 監査、品質改善、誤回答調査 |
| AI Tutorのrequest status、provider、model、prompt template version、guardrail結果、latency、token usage | backend | 障害調査、費用監視、品質改善 |
| 回答履歴、復習予定、自信度、理由タグ | 端末内 | 復習キュー表示 |
| 誤植報告 | 端末内、`TYPO_REPORT_API_URL` 有効buildではstaging backend | beta中の問題品質確認。対象箇所、対象テキスト、ユーザーコメントを扱う |
| betaフィードバック | Google Forms | 任意で入力した、初回体験、演習、履歴入力、復習、AI Tutor、Paywall、誤植報告、出典・監修表示への意見を扱う |

Sign in with Appleでは氏名とメールアドレスのscopeを要求せず、アプリ独自のDBへ保存しません。
アプリには、氏名、メールアドレス、電話番号、住所、位置情報、連絡先、写真、カレンダー、決済情報の専用入力欄を設けず、これらを初回TestFlightで意図的に収集しません。
ただし、AI Tutorの質問文、誤植報告、betaフィードバックの自由入力欄には、入力した内容がそのまま含まれます。氏名、Firebase UID、メールアドレス、問題文などの個人情報や機微な情報を入力しないでください。
現行backendは、AI Tutorの質問文とbackendへ送信した誤植報告を自動でPII検出またはredactionしません。
Google Formsではメールアドレスを自動収集しません。
実課金、購入復元、Sign in with Appleの操作導線は初回TestFlightには含みません。

## 利用目的

収集した情報は、以下の目的で利用します。

- AI Tutorへの質問に回答するため
- AI Tutorのrate limitと濫用防止を行うため
- Apple認証を有効にした実課金検証buildで、購入者のFirebase UIDと購入権利を同じApple Accountから復旧するため
- 不具合、エラー、根拠不足回答、ガードレール違反を調査するため
- betaテスターからの問い合わせ、誤植報告に対応するため
- 問題演習、復習キュー、AI Tutor導線の品質を改善するため

## 第三者サービス

TestFlight betaでは、以下の第三者サービスを利用します。Sign in with Appleは、初回buildでは開始せず、Apple認証を有効にした後続の実課金検証buildだけで利用します。

| サービス | 用途 |
|---|---|
| Firebase Authentication | 共通の匿名認証セッション、ID token発行。実課金検証buildではSign in with Appleとの連携も扱う |
| Sign in with Apple | Apple認証を有効にした実課金検証buildでの購入前認証と購入者アカウントの復旧 |
| Google Cloud Run / Cloud SQL | backend API、監査ログ、AI Tutorのユーザー・プラン別日次rate limit、`TYPO_REPORT_API_URL` 有効buildの誤植報告保存 |
| LLM provider API | AI Tutor回答生成。質問文、学習状態、問題本文、正誤を含む選択肢、解説、出典情報を送信する |
| Google Forms | betaフィードバックの受付・保存 |
| TestFlight / App Store Connect | beta配布 |

LLM provider API key、database接続情報、backend secretはモバイルアプリに含めません。

## 保存期間

現行backendは、AI Tutorの質問文、回答、引用情報、参照した問題ID、content version、監修状態、参照元ID、request status、provider、model、prompt template version、guardrail結果、latency、token usage、セキュリティ関連イベントを監査ログへ保存します。backendが参照コンテキストとして組み立てた問題本文、選択肢、解説、出典情報そのものは、独立した監査ログ項目として保存しません。
質問文などを180日、集計metadataなどを13か月で削除または匿名化する方針は設計目標ですが、自動削除処理と確認済みの手動運用は未実装です。そのため、現時点ではこれらの最長保存期間を保証しません。
削除処理、実行頻度、失敗時の監視、実行結果の証跡を用意するまで、初回TestFlight配布のblockerとします。この作業を扱うIssueはありません（後続Issue未作成）。

端末内に保存される回答履歴、復習予定、未送信の誤植報告は、ユーザーがアプリを削除するか、後続機能で削除手段が追加されるまで端末内に残ります。
`TYPO_REPORT_API_URL` 有効buildでbackendへ送信された誤植報告と、それに紐付くFirebase UID由来のbackend内ユーザーIDは、現行backendのDBへ保存されます。180日で削除または匿名化する方針は設計目標ですが、保持期間に基づく削除処理は未実装であり、現時点では最長保存期間を保証しません。この作業を扱うIssueはありません（後続Issue未作成）。
AI Tutorのユーザー・プラン別日次rate limitでは、Firebase UIDに紐づくbackend内ユーザーID、action、plan、UTC日付、request count、更新日時をCloud SQLへ保存します。保持期間に基づく削除処理は未実装であり、現時点では最長保存期間を保証しません。
Firebase UIDは、AI Tutor認証とアカウント管理に必要な期間、Firebase Authenticationで保持します。
実課金検証buildで作成したApple認証との連携情報は、購入権利の復旧とアカウント管理に必要な期間保持します。
Firebase Authenticationは認証処理でIPアドレスとuser agentを取り扱います。Googleの説明では、IPアドレスを記録する場合は数週間保持し、アカウント削除開始後も認証データが稼働系とbackupから削除されるまで最大180日かかる場合があります。詳細は[Firebaseのプライバシー情報](https://firebase.google.com/support/privacy/)を確認してください。
backendのin-memory rate limiterはIPアドレスを既定60秒のwindowで保持し、DBへ保存しません。一方、FastifyとCloud Loggingのrequest logにIPアドレスが含まれるか、含まれる場合の保持期間は未確認です。ログ内容と保持設定を確認するまで初回TestFlight配布のblockerとし、この作業を扱うIssueはありません（後続Issue未作成）。

Google Formsへ任意で送信されたbetaフィードバックは、開発関係者が閲覧し、フォームの提供・保存に必要な範囲でGoogleが取り扱います。180日で削除または匿名化する方針は設計目標ですが、確認済みの削除運用はありません。運用を用意するまで最長保存期間を保証せず、初回TestFlight配布のblockerとします。この作業を扱うIssueはありません（後続Issue未作成）。

## 共有と販売

収集した情報を広告目的で販売または共有しません。法令上必要な場合、サービス運用に必要な委託先、または不正利用調査に必要な範囲を除き、第三者へ提供しません。

## ユーザーの選択

Sign in with Appleを行わずに、問題演習、回答履歴、復習キューを利用できます。
Apple認証を有効にした実課金検証buildで認証をキャンセルした場合は、購入または購入復元を開始しません。
AI Tutorを利用した場合、質問文と関連する監査ログがbackendに保存されます。
誤植報告は任意で、`TYPO_REPORT_API_URL` 有効buildでは送信した報告内容がstaging backendに保存されます。
betaフィードバックも任意で、送信時は外部のGoogle Formsが開きます。

## 問い合わせ

TestFlight betaに関する一般的な問い合わせと不具合報告は、以下の公開サポートページから連絡してください。

https://shunki-235.github.io/cpa-short-answer-support/

公開Issueには、Firebase UIDを含む個人情報や機微な情報を書き込まないでください。
現在の公開サポートページには公開GitHub Issue以外の受付経路がなく、Firebaseアカウントを安全に特定して削除依頼を受け付ける手順も未整備です。そのため、公開IssueからFirebaseアカウントの削除を依頼することはできません。
初回TestFlight配布前に、非公開の削除依頼受付、対象アカウントの確認、Firebaseとbackendの関連データを削除する手順、完了連絡の方法を用意します。このbeta向け運用を扱うIssueはありません（後続Issue未作成）。
一般App Store公開前のアプリ内削除開始導線、関連データ削除、Apple連携済みの場合のtoken失効処理は、OPENの[#51](https://github.com/shunki-235/CPA-Short-Answer-Exam/issues/51)で扱います。
