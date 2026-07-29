---
layout: page
title: CPA短答トレーナー TestFlightプライバシーポリシー
permalink: /privacy/
---

最終更新日: 2026-07-29

このプライバシーポリシーは、CPA短答トレーナーのTestFlight betaに適用するための草案です。本アプリは公認会計士試験・短答式試験の学習支援を目的とした非公式アプリであり、試験実施機関または関係団体が提供、承認、監修する公式アプリではありません。
2026-07-25時点で、backendログのIPアドレスとuser agentの取扱いを確認し、staging backendでは新規のCloud Run `run.googleapis.com/requests` logを`_Default` bucketへ保存しない設定と、ネットワーク識別情報を記録しないFastifyログへ変更しました。
backendログに関する変更は公開ページにも反映済みです。
AI Tutor監査ログ、日次利用カウンター、誤植報告、Google Forms回答の保持期間と削除方法を決定し、stagingの定期処理へ反映しました。
Cloud SQLの期限境界、失敗時のrollbackと再実行、Google Formsの期限超過回答削除を検証し、Cloud Schedulerと二つのApps Script日次triggerを有効にしています。
この保持期間改訂も公開ページへ反映済みです。
TestFlight beta専用のアカウント削除依頼フォームを作成し、iOS Simulator上のアプリから確認コードを発行して実フォームを開き、検証用の依頼1件を送信できることを確認しました。
同フォームの保持処理に使うApps ScriptへOAuthを許可し、対象0件、削除0件、失敗0件のpreviewと本実行、安全な実行履歴、26時間の成功鮮度監視、二つの日次triggerを確認しました。
一時copyを使う期限超過回答の削除、再実行、連携Sheet検出時の停止と復旧は未完了です。
削除operatorによる同依頼の処理、完了連絡、未解決alertの実着信も未確認です。
これらの運用確認と公開ページへの反映を終えるまで、この草案をbackendへ接続する追加配布用の最終ポリシーとして扱いません。

## 収集する情報

初回TestFlightでは、以下の情報を扱います。
購入UIを含まない初回buildでは匿名Firebase認証だけを使い、Sign in with Appleを開始しません。
後続の実課金検証buildでApple認証を有効にした場合だけ、表の「実課金検証build」と記した情報を追加で扱います。

| 情報 | 取扱場所 | 用途 |
|---|---|---|
| Firebase UID | Firebase、backend | 匿名ユーザーまたはApple連携済みユーザーの識別、AI Tutorの認証、rate limit、濫用防止。`TYPO_REPORT_API_URL` 有効buildでは誤植報告の送信者識別にも使う |
| Apple Accountの認証識別子 | Apple、Firebase | 実課金検証buildで、購入前の本人確認と再インストール後のFirebase UID、購入権利の復旧に使う |
| IPアドレス、user agent | Firebase Authentication。IPアドレスはbackendのin-memory IP rate limiterでも処理する。Cloud Runはrequest log生成時にIPアドレスとuser agentを処理する | 認証要求の処理、不正利用防止、セキュリティ保護。backendのIP rate limiterはIPアドレスをDBへ保存しない。staging backendのCloud Run `run.googleapis.com/requests` logは`_Default` bucketの保存対象から除外し、Fastifyはネットワーク識別情報をapplication logへ記録しない |
| AI Tutorの利用回数と利用プラン | backendのCloud SQL | Firebase UIDに紐づくbackend内ユーザーID、action、plan、UTC日付、request count、更新日時を使い、ユーザー・プラン別の日次rate limitを適用する |
| AI Tutorへの質問文 | backend、LLM provider API | 回答生成、品質確認、問い合わせ対応 |
| AI Tutorの回答、引用情報、参照した問題ID、content version、監修状態、参照元ID | backend | 監査、品質改善、誤回答調査 |
| AI Tutorのrequest status、provider、model、prompt template version、guardrail結果、latency、token usage | backend | 障害調査、費用監視、品質改善 |
| 回答履歴、復習予定、自信度、理由タグ | 端末内 | 復習キュー表示 |
| 誤植報告 | 端末内、`TYPO_REPORT_API_URL` 有効buildではstaging backend | beta中の問題品質確認。対象箇所、対象テキスト、ユーザーコメントを扱う |
| betaフィードバック | Google Forms | 任意で入力した、初回体験、演習、履歴入力、復習、AI Tutor、Paywall、誤植報告、出典・監修表示への意見を扱う |
| TestFlight betaのアカウント削除フォーム回答 | Google Forms | アプリが発行した確認コード、完了連絡先メールアドレス、アカウント種別、削除への同意を使い、削除対象の確認、削除作業、完了連絡を行う |
| TestFlight betaのアカウント削除処理台帳 | backend | 確認コードのhash、Firebase UIDのhash、処理状態、処理時刻、完了連絡の記録時刻、安全な固定failure codeを使い、削除の進行、失敗からの再実行、完了連絡の記録を行う。確認コードそのものと完了連絡先メールアドレスは保存しない |
| TestFlight betaのアカウント削除完了メール | 運用者のメールproviderと送信済みメールbox | フォームへ入力された完了連絡先メールアドレスへ、削除完了、端末内データ、backupの削除期間を連絡する |

Sign in with Appleでは氏名とメールアドレスのscopeを要求せず、アプリ独自のDBへ保存しません。
アプリには、氏名、メールアドレス、電話番号、住所、位置情報、連絡先、写真、カレンダー、決済情報の専用入力欄を設けず、これらを初回TestFlightで意図的に収集しません。
ただし、AI Tutorの質問文、誤植報告、betaフィードバックの自由入力欄には、入力した内容がそのまま含まれます。氏名、Firebase UID、メールアドレス、問題文などの個人情報や機微な情報を入力しないでください。
現行backendは、AI Tutorの質問文とbackendへ送信した誤植報告を自動でPII検出またはredactionしません。
Google Formsではメールアドレスを自動収集しません。
アカウント削除依頼フォームでは、完了連絡先メールアドレスを必須の専用設問として入力してもらいます。
同フォームへFirebase UID、ID token、Apple Account情報、パスワードを入力しないでください。
実課金、購入復元、Sign in with Appleの操作導線は初回TestFlightには含みません。

## 利用目的

収集した情報は、以下の目的で利用します。

- AI Tutorへの質問に回答するため
- AI Tutorのrate limitと濫用防止を行うため
- Apple認証を有効にした実課金検証buildで、購入者のFirebase UIDと購入権利を同じApple Accountから復旧するため
- 不具合、エラー、根拠不足回答、ガードレール違反を調査するため
- betaテスターからの問い合わせ、誤植報告に対応するため
- 問題演習、復習キュー、AI Tutor導線の品質を改善するため
- アカウントの削除対象を確認し、削除処理を行い、完了を連絡するため

## 第三者サービス

TestFlight betaでは、以下の第三者サービスを利用します。Sign in with Appleは、初回buildでは開始せず、Apple認証を有効にした後続の実課金検証buildだけで利用します。

| サービス | 用途 |
|---|---|
| Firebase Authentication | 共通の匿名認証セッション、ID token発行。実課金検証buildではSign in with Appleとの連携も扱う |
| Sign in with Apple | Apple認証を有効にした実課金検証buildでの購入前認証と購入者アカウントの復旧 |
| Google Cloud Run / Cloud SQL | backend API、監査ログ、AI Tutorのユーザー・プラン別日次rate limit、`TYPO_REPORT_API_URL` 有効buildの誤植報告保存 |
| LLM provider API | AI Tutor回答生成。質問文、学習状態、問題本文、正誤を含む選択肢、解説、出典情報を送信する |
| Google Forms | betaフィードバックとTestFlight betaのアカウント削除依頼の受付・保存 |
| 運用者のメールprovider（Gmail） | TestFlight betaのアカウント削除完了メールの送信と保存。完了連絡先メールアドレスと、識別情報を含まない定型の完了内容を扱う |
| TestFlight / App Store Connect | beta配布 |

LLM provider API key、database接続情報、backend secretはモバイルアプリに含めません。

## 保存期間

個人情報保護法は個人情報の保存期間を一律には定めていません。
[個人情報保護委員会のFAQ](https://www.ppc.go.jp/all_faq_index/faq1-q5-2/)は、利用する必要がなくなった個人データを遅滞なく消去するよう努める必要があると説明しています。
本betaでは、利用目的に必要な期間と自由入力を長く残すリスクを踏まえ、次の保持期間を定めます。

| 情報 | 起算点 | 保持期限後の処理 |
|---|---|---|
| AI Tutorの質問文、回答、引用情報、参照した問題ID、content version、監修状態、参照元ID | 監査recordの作成日時 | 180日経過後の次の日次処理で、該当項目をNULLにする |
| AI Tutorのrequest ID、backend内ユーザーIDとの紐付け、mode、request status、provider、model、prompt template version、guardrail結果、latency、token usage | 監査recordの作成日時 | 13か月経過後の次の日次処理で、監査record全体を削除する |
| AI Tutorのユーザー・プラン別日次利用カウンター | UTCの利用日 | 31日経過後の次の日次処理で、record全体を削除する |
| backendへ送信した誤植報告 | backendの受信日時 | 180日経過後の次の日次処理で、record全体を削除する |
| Google Formsのbetaフィードバック | Google Formsが記録した送信日時 | 180日経過後の次の日次処理で、回答全体を削除する |
| Google Formsのアカウント削除依頼 | Google Formsが記録した送信日時 | 180日経過後の次の日次処理で、回答全体を削除する |
| backendのアカウント削除処理台帳 | 未開始の場合は作成日時、完了した場合は完了連絡の記録日時 | 181日経過後の次の日次処理で、record全体を削除する。開始後に`processing`または`failed`となった未完了のrecordは、failure codeの有無にかかわらず復旧が完了するまで保持する |
| Firebase UIDのhashによる削除tombstone | 削除完了日時 | 181日経過後の次の日次処理で、record全体を削除する。削除が未完了の場合は、復旧が完了するまで保持する |
| 送信済みのアカウント削除完了メール | メール送信日時 | 運用者の送信済みメールboxでは180日経過後に削除する。受信者側とメールproviderのbackupは、それぞれの保持・削除手順に従う |

AI Tutorの内容をNULLにした後も、費用監視、障害調査、guardrailと濫用傾向の確認に必要なmetadataは、record作成から13か月まで残ります。
13か月を過ぎると、backend内ユーザーIDとの紐付けを含むrecord全体を削除します。
backendが参照コンテキストとして組み立てた問題本文、選択肢、解説、出典情報そのものは、独立した監査ログ項目として保存しません。
日次利用カウンターは当日のrate limit適用と直近の濫用調査にだけ使うため、31日とします。
誤植報告は端末が記録した`reported_at`ではなく、オフライン送信の遅れや端末時刻の差に影響されないbackend側の`received_at`から数えます。
期限と同時刻または同日付のrecordは、その次の日次処理で対象になります。

端末内に保存される回答履歴、復習予定、未送信の誤植報告は、ユーザーがアプリを削除するか、後続機能で削除手段が追加されるまで端末内に残ります。
Firebase UIDは、AI Tutor認証とアカウント管理に必要な期間、Firebase Authenticationで保持します。
実課金検証buildで作成したApple認証との連携情報は、購入権利の復旧とアカウント管理に必要な期間保持します。
Firebase Authenticationは認証処理でIPアドレスとuser agentを取り扱います。Googleの説明では、IPアドレスを記録する場合は数週間保持し、アカウント削除開始後も認証データが稼働系とbackupから削除されるまで最大180日かかる場合があります。詳細は[Firebaseのプライバシー情報](https://firebase.google.com/support/privacy/)を確認してください。
backendのin-memory rate limiterはIPアドレスを既定60秒のwindowで保持し、DBへ保存しません。
2026-07-25の確認では、Cloud Runが自動生成したrequest logにIPアドレスとuser agentがあり、Fastifyの標準request logにも接続元IPアドレスがありました。
staging backendのCloud Run `run.googleapis.com/requests` logは、同日から`_Default` bucketの保存対象から除外しています。
Fastifyは標準request logを使わず、request ID、HTTP method、route template、status code、処理時間だけを記録する構成へ変更し、revision `ai-tutor-api-staging-2fb798d`へ反映しました。
ダミーのIPアドレス、user agent、Authorization、query文字列を付けたstaging requestを使い、これらがFastifyの完了ログに保存されず、Cloud Run request logも`_Default` bucketへ保存されないことを確認しました。
この確認は`_Default` bucketへの保存を対象とし、他のユーザー定義bucketや転送先を網羅したものではありません。2026-07-25時点では、追加のexport sinkがないことも確認しました。
除外追加前のCloud Run `run.googleapis.com/requests` logは、`_Default` bucketの既定の30日保持期間に従って残ります。
変更前のFastify標準request logはcontainer logとして`_Default` bucketに保存され、同じ30日保持期間に従って残ります。
変更後のFastify完了ログとerror logもcontainer logとして`_Default` bucketに保存されます。これらはCloud Run request logの除外対象ではなく、既定の30日保持期間が適用されます。
確認内容、検索条件、rollback手順は、完了済みの[#151](https://github.com/shunki-235/CPA-Short-Answer-Exam/issues/151)で記録しています。

Google Formsへ任意で送信されたbetaフィードバックは、フォームowner 1名が閲覧し、フォームの提供と保存に必要な範囲でGoogleが取り扱います。
2026-07-25時点ではGoogle Formsのresponse storeだけに保存し、Google Sheetsとは連携していません。
同日時点の回答は2026-07-12送信の1件で、180日の保持期間内です。

TestFlight betaのアカウント削除依頼も、別のGoogle Formでフォームowner 1名が閲覧します。
2026-07-29時点で、リンクを知る人がGoogleログインなしで回答でき、メールアドレスの自動収集、回答先のGoogle Sheets、回答コピー、送信後の編集は設定していません。
同日時点の検証用回答は1件です。
backendには確認コードそのものと完了連絡先メールアドレスを保存せず、確認コードのhash、Firebase UIDのhash、処理状態、処理時刻、完了連絡の記録時刻、安全な固定failure codeだけを処理台帳へ保存します。
完了メールには、確認コード、Firebase UID、backend内ユーザーID、Apple Account情報を含めません。

2026-07-25時点では、Cloud SQLの自動backupとPoint-in-time recoveryは無効です。
対象データをCloud StorageまたはBigQueryへarchiveする処理と、対象recordを複製する外部sinkまたはexportはありません。
Google Formsの連携Sheetもありません。

Cloud SQLの日次処理は、専用接続権限だけを持つCloud Run JobをCloud Schedulerが毎日起動します。
stagingでは期限境界、dry-run、本実行、同じ基準時刻での再実行、失敗時のrollback、復旧後の再実行を確認しました。
Google Formsでは、権限を限定したApps Scriptが保持処理と26時間成功なし監視を毎日実行します。
一時copyの非個人情報回答を使い、期限超過回答1件の削除、再実行0件、連携Sheet検出時の安全な停止と解除後の復旧を確認しました。
確認手順と結果は[#150](https://github.com/shunki-235/CPA-Short-Answer-Exam/issues/150)で記録しています。

アカウント削除依頼フォーム用のApps Scriptは、実フォームでOAuth許可、preview、本実行、安全な実行履歴、成功鮮度監視、二つの日次triggerまで確認済みです。
一時copyによる期限超過回答の削除、再実行、連携Sheet検出時の停止と復旧は未完了です。

## 共有と販売

収集した情報を広告目的で販売または共有しません。法令上必要な場合、サービス運用に必要な委託先、または不正利用調査に必要な範囲を除き、第三者へ提供しません。

## ユーザーの選択

Sign in with Appleを行わずに、問題演習、回答履歴、復習キューを利用できます。
Apple認証を有効にした実課金検証buildで認証をキャンセルした場合は、購入または購入復元を開始しません。
AI Tutorを利用した場合、質問文と関連する監査ログがbackendに保存されます。
誤植報告は任意で、`TYPO_REPORT_API_URL` 有効buildでは送信した報告内容がstaging backendに保存されます。
betaフィードバックも任意で、送信時は外部のGoogle Formsが開きます。
Firebaseアカウントとbackend上の関連データの削除を希望する場合は、この導線を設定した対象buildの配布後、アプリの「TestFlight アカウント削除」から確認コードを発行し、表示された専用フォームへ送信できます。

## 問い合わせ

TestFlight betaに関する一般的な問い合わせと不具合報告は、以下の公開サポートページから連絡してください。

https://shunki-235.github.io/cpa-short-answer-support/

公開Issueには、Firebase UIDを含む個人情報や機微な情報を書き込まないでください。
公開IssueではFirebaseアカウントの削除依頼を受け付けません。
TestFlight beta参加者には、対象buildのアプリ内からだけ非公開の削除依頼フォームを案内します。
[PR #149](https://github.com/shunki-235/CPA-Short-Answer-Exam/pull/149)以降にbackendへ接続するbuildを追加配布する前に、同フォームの保持処理、対象アカウントの削除、完了連絡、未解決alertの実着信を確認します。
このbeta向け運用はOPENの[#152](https://github.com/shunki-235/CPA-Short-Answer-Exam/issues/152)で扱い、[#150](https://github.com/shunki-235/CPA-Short-Answer-Exam/issues/150)の定期保持処理とは別に確認します。
一般App Store公開前のアプリ内削除開始導線、関連データ削除、Apple連携済みの場合のtoken失効処理は、OPENの[#51](https://github.com/shunki-235/CPA-Short-Answer-Exam/issues/51)で扱います。
