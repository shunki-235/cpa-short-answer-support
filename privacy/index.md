---
layout: page
title: CPA短答トレーナー プライバシーポリシー
permalink: /privacy/
---

最終更新日: 2026-08-26

このプライバシーポリシーは、CPA短答トレーナーのTestFlight版とApp Store版に適用します。
本アプリは公認会計士試験、短答式試験の学習支援を目的とした非公式アプリであり、試験実施機関または関係団体が提供、承認、監修する公式アプリではありません。
2026-07-25時点で、backendログのIPアドレスとuser agentの取扱いを確認し、staging backendでは新規のCloud Run `run.googleapis.com/requests` logを`_Default` bucketへ保存しない設定と、ネットワーク識別情報を記録しないFastifyログへ変更しました。
backendログに関する変更は公開ページにも反映済みです。
AI Tutor監査ログ、日次利用カウンター、誤植報告、Google Forms回答の保持期間と削除方法を決定し、stagingの定期処理へ反映しました。
Cloud SQLの期限境界、失敗時のrollbackと再実行、Google Formsの期限超過回答削除を検証し、Cloud Schedulerと二つのApps Script日次triggerを有効にしています。
この保持期間改訂も公開ページへ反映済みです。
TestFlight beta専用のアカウント削除依頼フォームを作成し、iOS Simulator上のアプリから確認コードを発行して実フォームを開き、検証用の依頼1件を送信できることを確認しました。
同フォームの保持処理に使うApps ScriptへOAuthを許可し、対象0件、削除0件、失敗0件のpreviewと本実行、安全な実行履歴、26時間の成功鮮度監視、二つの日次triggerを確認しました。
一時copyを使い、期限超過回答1件の削除、再実行0件、連携Sheet検出時の安全な停止と回答保持、連携解除後の削除を確認しました。
実フォームの依頼は削除operatorで処理し、Firebase利用者1件とbackendの`users` 1件を削除して`completed`となること、完了メールの送受信、通知済み記録を確認しました。
未解決alertは、専用fixture 3件中2件が保持Jobの元ログへ記録され、log-based metricに値1の`INT64` pointが作成され、incidentが`OPEN`となることを確認しました。
Gmailへのalertメール受信、fixture cleanup、後続inspectの未解決0件、最終保持Jobの未解決0件と失敗0件まで確認しました。
0件ログ後の5分のalignment window終了後にincident詳細を再読み込みし、alertが自動close済みであることを確認しました。
incidentはすでにclosedであったため、手動のpermanent closeは実施していません。
このフォーム経路は、自己完結削除を持たない初期TestFlight buildの運用として維持します。
2026-08-26に、一般公開向け自己完結削除とRevenueCat有効buildの取扱いをこのポリシーへ追加しました。

## 収集する情報

利用する機能とbuildに応じて、以下の情報を扱います。
自己完結削除とRevenueCatを有効にしたbuildでは、Apple認証、購入、利用権限、アカウント削除に必要な行も対象になります。

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
| 自己完結削除のreceipt | 利用者端末、backend | Firebase認証の削除後も同じ削除requestの状態確認を再開するために使う。端末はraw receiptと有効期限、backendは用途を分離したhashと有効期限だけを保存する |
| Apple authorization codeとID token | 利用者端末からbackendを経由してApple | Apple連携済み利用者の再認証とApple refresh tokenの失効に使う。authorization code、ID token、refresh tokenは端末ファイル、backend DB、application logへ保存しない |
| RevenueCat App User ID mapping | backend、RevenueCat | Firebase利用者と購入権利を対応付け、webhook処理、購入状態の再照合、customer削除に使う。raw Firebase UIDはRevenueCatへ送らず、backendが生成した不透明なApp User IDを使う |
| RevenueCat SDKの技術情報 | RevenueCat | 端末種別、OS、app version、SDK version、locale、currency、最終接続時刻、ATT consent status、取引国またはIPアドレスから推定した国を、SDK接続、購入検証、障害調査に使う。本アプリは氏名、メールアドレス、IDFA、IDFV、広告、attributionのcustomer属性を追加設定しない |
| 購入と利用権限の情報 | Apple、RevenueCat、backend | product、entitlement、transactionの識別子、購入日時、有効期限、取消、返金、失効、復元、価格、通貨、国、取引環境を、購入確認、利用権限の判定、復元、問い合わせ、重複処理防止に使う。カード番号やApple Accountの決済credentialはAppleが扱い、本アプリのbackendへ提供しない |
| 削除処理台帳とtombstone | backend | 削除の進行、失敗からの再開、削除済みidentityの再接続拒否に使う。Firebase UID、RevenueCat project、customer ID、aliasは用途を分離したhashで保存し、raw値をtombstoneへ保存しない |

Sign in with Appleでは氏名とメールアドレスのscopeを要求せず、アプリ独自のDBへ保存しません。
アプリ本体には、氏名、メールアドレス、電話番号、住所、位置情報、連絡先、写真、カレンダー、決済情報の専用入力欄を設けず、これらを意図的に収集しません。
ただし、アカウント削除依頼を送信する場合だけ、アプリ外の専用フォームで完了連絡先メールアドレスを収集します。
ただし、AI Tutorの質問文、誤植報告、betaフィードバックの自由入力欄には、入力した内容がそのまま含まれます。氏名、Firebase UID、メールアドレス、問題文などの個人情報や機微な情報を入力しないでください。
現行backendは、AI Tutorの質問文とbackendへ送信した誤植報告を自動でPII検出またはredactionしません。
Google Formsではメールアドレスを自動収集しません。
アカウント削除依頼フォームでは、完了連絡先メールアドレスを必須の専用設問として入力してもらいます。
同フォームへFirebase UID、ID token、Apple Account情報、パスワードを入力しないでください。
RevenueCat SDKは、自動device identifier収集とdiagnosticsを無効にしてから初期化します。
custom log handlerをSDK初期化前に設定し、RevenueCatのnative logをapplication logへ転送しません。

## 利用目的

収集した情報は、以下の目的で利用します。

- AI Tutorへの質問に回答するため
- AI Tutorのrate limitと濫用防止を行うため
- 利用者を認証し、同じApple Accountからアカウントと購入権利を復旧するため
- 購入、復元、返金、利用権限を確認するため
- 不具合、エラー、根拠不足回答、ガードレール違反を調査するため
- betaテスターからの問い合わせ、誤植報告に対応するため
- 問題演習、復習キュー、AI Tutor導線の品質を改善するため
- アカウントと関連データの削除を開始し、失敗から再開して完了を確認するため

## 第三者サービス

本アプリは、利用する機能とbuildに応じて以下の第三者サービスを利用します。

| サービス | 用途 |
|---|---|
| Firebase Authentication | 匿名認証、Sign in with Appleとの連携、ID token発行、利用者削除 |
| Sign in with Apple | 購入前の本人確認、アカウント復旧、Apple連携済みアカウント削除時の再認証とtoken失効 |
| Google Cloud Run / Cloud SQL | backend API、監査ログ、rate limit、誤植報告、課金情報、削除処理台帳の保存と処理 |
| LLM provider API | AI Tutor回答生成。質問文、学習状態、問題本文、正誤を含む選択肢、解説、出典情報を送信する |
| Google Forms | betaフィードバックとTestFlight betaのアカウント削除依頼の受付・保存 |
| 運用者のメールprovider（Gmail） | TestFlight betaのアカウント削除完了メールの送信と保存。完了連絡先メールアドレスと、識別情報を含まない定型の完了内容を扱う |
| TestFlight / App Store Connect | アプリのbeta配布と一般配布 |
| RevenueCat | App Store購入の検証、購入状態と利用権限の管理、復元、返金、customer削除 |

LLM provider API key、database接続情報、backend secretはモバイルアプリに含めません。

[RevenueCat Privacy Policy](https://www.revenuecat.com/privacy/)も確認してください。
RevenueCat customerの削除後も、同じApp User IDを保持する旧buildや別端末が通信すると、購入や利用権限を持たない空のcustomerが再作成される場合があります。
backendは削除済みidentityを再発行せず、再作成されたcustomerを利用者や利用権限へ結び付けません。

## 保存期間

個人情報保護法は個人情報の保存期間を一律には定めていません。
[個人情報保護委員会のFAQ](https://www.ppc.go.jp/all_faq_index/faq1-q5-2/)は、利用する必要がなくなった個人データを遅滞なく消去するよう努める必要があると説明しています。
本アプリでは、利用目的に必要な期間と自由入力を長く残すリスクを踏まえ、次の保持期間を定めます。

| 情報 | 起算点 | 保持期限後の処理 |
|---|---|---|
| AI Tutorの質問文、回答、引用情報、参照した問題ID、content version、監修状態、参照元ID | 監査recordの作成日時 | 180日経過後の次の日次処理で、該当項目をNULLにする |
| AI Tutorのrequest ID、backend内ユーザーIDとの紐付け、mode、request status、provider、model、prompt template version、guardrail結果、latency、token usage | 監査recordの作成日時 | 13か月経過後の次の日次処理で、監査record全体を削除する |
| AI Tutorのユーザー・プラン別日次利用カウンター | UTCの利用日 | 31日経過後の次の日次処理で、record全体を削除する |
| backendへ送信した誤植報告 | backendの受信日時 | 180日経過後の次の日次処理で、record全体を削除する |
| Google Formsのbetaフィードバック | Google Formsが記録した送信日時 | 180日経過後の次の日次処理で、回答全体を削除する |
| Google Formsのアカウント削除依頼 | Google Formsが記録した送信日時 | 180日経過後の次の日次処理で、回答全体を削除する |
| backendのアカウント削除処理台帳 | 未開始の場合は作成日時、完了した場合は完了連絡の記録日時 | 181日経過後の次の日次処理で、record全体を削除する。開始後に`processing`または`failed`となった未完了のrecordは、failure codeの有無にかかわらず復旧が完了するまで保持する |
| 自己完結削除のraw receipt | 発行日時 | 発行から30日で失効する。アプリは完了または失効の確認後に端末から削除する |
| Firebase UIDのhashによる削除tombstone | 削除完了日時 | 181日経過後の次の日次処理で、record全体を削除する。削除が未完了の場合は、復旧が完了するまで保持する |
| RevenueCat App User ID mappingとbackendのactiveな課金record | mappingまたはrecordの作成日時 | アカウントの存在中は購入確認と利用権限判定に必要な範囲で保持する。アカウント削除時はRevenueCat customerの削除を確認した後に削除する |
| RevenueCat customer、SDK技術情報、購入履歴 | customer作成日時、最終接続日時、取引日時 | アカウントの存在中は購入確認、復元、障害調査に必要な範囲でRevenueCatが保持する。アカウント削除時はcustomerを削除し、customer取得が404になることを確認する。法令上必要な記録とbackupにはRevenueCatの保持規則が適用される |
| RevenueCat identityのhashによる削除tombstone | customer削除開始前の作成日時または削除完了日時 | raw customer IDとaliasを保存せず、未完了では復旧完了まで保持する。削除完了後181日で削除する |
| 送信済みのアカウント削除完了メール | メール送信日時 | 運用者の送信済みメールboxでは180日経過後に削除する。受信者側とメールproviderのbackupは、それぞれの保持・削除手順に従う |

AI Tutorの内容をNULLにした後も、費用監視、障害調査、guardrailと濫用傾向の確認に必要なmetadataは、record作成から13か月まで残ります。
13か月を過ぎると、backend内ユーザーIDとの紐付けを含むrecord全体を削除します。
backendが参照コンテキストとして組み立てた問題本文、選択肢、解説、出典情報そのものは、独立した監査ログ項目として保存しません。
日次利用カウンターは当日のrate limit適用と直近の濫用調査にだけ使うため、31日とします。
誤植報告は端末が記録した`reported_at`ではなく、オフライン送信の遅れや端末時刻の差に影響されないbackend側の`received_at`から数えます。
期限と同時刻または同日付のrecordは、その次の日次処理で対象になります。

端末内に保存される回答履歴、復習予定、未送信の誤植報告は、自己完結削除の完了後にアプリが削除します。
アカウント削除を行わない場合は、利用者がアプリを削除するまで端末内に残ります。
Firebase UIDは、AI Tutor認証とアカウント管理に必要な期間、Firebase Authenticationで保持します。
Apple認証との連携情報は、購入権利の復旧とアカウント管理に必要な期間保持します。
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
同日時点の検証用回答1件を削除operatorで処理し、完了メールの送受信と通知済み記録を確認しました。
backendには確認コードそのものと完了連絡先メールアドレスを保存せず、確認コードのhash、Firebase UIDのhash、処理状態、処理時刻、完了連絡の記録時刻、安全な固定failure codeだけを処理台帳へ保存します。
完了メールには、確認コード、Firebase UID、backend内ユーザーID、Apple Account情報を含めません。

2026-07-25のstaging確認時点では、Cloud SQLの自動backupとPoint-in-time recoveryは無効でした。
productionを含むbackend backup内の削除済みrowは個別に選択して削除できず、Google Cloudの設定とbackup lifecycleに従います。
対象データをCloud StorageまたはBigQueryへ恒常的に複製する外部sinkまたはexportはありません。
Google Formsの連携Sheetもありません。

Cloud SQLの日次処理は、専用接続権限だけを持つCloud Run JobをCloud Schedulerが毎日起動します。
stagingでは期限境界、dry-run、本実行、同じ基準時刻での再実行、失敗時のrollback、復旧後の再実行を確認しました。
Google Formsでは、権限を限定したApps Scriptが保持処理と26時間成功なし監視を毎日実行します。
一時copyの非個人情報回答を使い、期限超過回答1件の削除、再実行0件、連携Sheet検出時の安全な停止と解除後の復旧を確認しました。
確認手順と結果は[#150](https://github.com/shunki-235/CPA-Short-Answer-Exam/issues/150)で記録しています。

アカウント削除依頼フォーム用のApps Scriptは、実フォームでOAuth許可、preview、本実行、安全な実行履歴、成功鮮度監視、二つの日次triggerまで確認済みです。
一時copyの非個人情報回答を使い、期限超過回答1件の削除、再実行0件、連携Sheet検出時の安全な停止と回答保持、連携解除後の削除を確認しました。
検証後は一時フォームとSheetをGoogle Driveのゴミ箱へ移動しました。

## アカウント削除

自己完結削除を有効にしたbuildでは、アプリ内の「アカウントを削除」から削除を開始できます。
匿名利用者は確認画面から開始できます。
Apple連携済み利用者は、同じApple Accountで再認証した後に開始できます。

削除処理は、RevenueCat SDKの購入、復元、同期、CustomerInfo更新を停止してから開始します。
Apple連携済みの場合は、Appleのauthorization codeを交換してrefresh tokenを失効します。
その後、RevenueCat customer、backendの利用者データ、Firebase Authenticationの利用者を削除します。
完了後は、端末内の回答履歴、復習予定、analytics event、未送信の誤植報告、認証sessionを削除します。

アカウント削除は、App Storeのサブスクリプション解約、Apple Accountの削除、Appleが保持する取引記録の削除ではありません。
有効な契約は、Apple Accountのサブスクリプション管理画面から別に解約してください。

自己完結削除を持たない初期TestFlight buildでは、アプリ内から表示される非公開フォームを利用できます。
公開Issueでは削除依頼を受け付けません。

## 共有と販売

収集した情報を広告目的で販売または共有しません。
法令上必要な場合、サービス運用に必要な委託先、または不正利用調査に必要な範囲を除き、第三者へ提供しません。
本アプリは、広告目的のtrackingを行いません。

## ユーザーの選択

Sign in with Appleを行わずに、問題演習、回答履歴、復習キューを利用できます。
Apple認証をキャンセルした場合は、購入または購入復元を開始しません。
AI Tutorを利用した場合、質問文と関連する監査ログがbackendに保存されます。
誤植報告は任意で、送信した報告内容がbackendに保存されます。
betaフィードバックも任意で、送信時は外部のGoogle Formsが開きます。
アカウント削除は、上記のアプリ内導線から開始できます。

## 問い合わせ

一般的な問い合わせと不具合報告は、以下の公開サポートページから連絡してください。

https://shunki-235.github.io/cpa-short-answer-support/

公開Issueには、Firebase UID、ID token、削除receipt、Apple Account情報、メールアドレスなどの個人情報、識別情報、認証情報を書き込まないでください。
公開IssueではFirebaseアカウントの削除依頼を受け付けません。
自己完結削除を有効にしたbuildでは、アプリ内の削除導線を利用してください。
初期TestFlight buildの参加者には、対象buildのアプリ内からだけ非公開の削除依頼フォームを案内します。
2026-07-29に、検証済みの削除受付と運用手順を[公開サポートPR #8](https://github.com/shunki-235/cpa-short-answer-support/pull/8)で公開ページへ同期しました。
このbeta向け運用は[#152](https://github.com/shunki-235/CPA-Short-Answer-Exam/issues/152)で確認し、[#150](https://github.com/shunki-235/CPA-Short-Answer-Exam/issues/150)の定期保持処理とは別に運用します。
一般公開向けのアプリ内削除開始導線、関連データ削除、Apple連携済みの場合のtoken失効、RevenueCat customer削除は、[#167](https://github.com/shunki-235/CPA-Short-Answer-Exam/issues/167)と子Issue[#185](https://github.com/shunki-235/CPA-Short-Answer-Exam/issues/185)で実装と検証を追跡しています。
