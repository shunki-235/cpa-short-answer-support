---
layout: page
title: CPA短答トレーナー プライバシーポリシー
permalink: /privacy/
---

最終更新日: 2026-09-02

このプライバシーポリシーは、CPA短答トレーナーのApp Store版とTestFlight版に適用します。本アプリは、公認会計士試験・短答式試験の学習支援を目的とする非公式アプリです。試験実施機関または関係団体が提供、承認、監修する公式アプリではありません。

## 取り扱う情報

| 情報 | 取扱場所 | 用途 |
|---|---|---|
| Firebase UID、backend内部の利用者ID | Firebase、backend | 認証、AI Tutorの利用回数制限、濫用防止、誤植報告、アカウント削除 |
| Apple Accountの認証識別子 | Apple、Firebase | 購入前の本人確認、再インストール後のアカウントと購入権利の復旧、削除時の再認証とtoken失効 |
| IPアドレス、user agent | Firebase Authentication、Cloud Run、backendの短時間メモリ | 認証要求の処理、rate limit、セキュリティ保護。backendのIP rate limiterはIPアドレスをDBへ保存しない |
| AI Tutorへの質問文、回答、引用、問題ID、content version、監修状態、参照元ID | backend、LLM provider API | 回答生成、品質確認、誤回答と問い合わせの調査 |
| AI Tutorのrequest ID、mode、status、provider、model、prompt template version、guardrail結果、latency、token usage、認可時点のplanと購入環境、日次利用回数 | backend | 障害調査、費用監視、品質改善、利用回数制限、濫用防止。購入環境は`production`、`sandbox`、`unknown`のいずれか |
| 回答履歴、復習予定、自信度、理由タグ、利用event | 端末内 | 学習画面と復習キューの表示、アプリ内の利用状況の記録。利用eventは外部analyticsサービスへ送信しない |
| 誤植報告 | 端末内、backend | 対象箇所、対象テキスト、任意のコメントを問題品質の改善に使う |
| RevenueCatの不透明なApp User ID、SDK技術情報、購入履歴、利用権限 | Apple、RevenueCat、backend | 購入確認、利用権限判定、復元、返金・失効の反映、重複処理防止、問い合わせ対応 |
| 自己完結削除のreceiptと削除処理台帳、削除tombstone | 端末内、backend | 削除状態の再開、失敗からの復旧、削除済みidentityの再接続防止。識別子は用途を分離したhashで保存する |
| TestFlight betaの任意フィードバックと旧削除依頼フォーム | Google Forms、運用者のメールprovider | beta品質確認、自己完結削除を持たない旧buildの削除依頼と完了連絡 |
| 特定商取引法に基づく表示の開示請求 | Google Forms、運用者のメールprovider | 返信先メールアドレスを使い、販売事業者の住所・電話番号を請求者へ個別に開示する |

Sign in with Appleでは氏名とメールアドレスのscopeを要求せず、アプリ独自のDBへ保存しません。アプリ本体には、氏名、メールアドレス、電話番号、住所、位置情報、連絡先、写真、カレンダー、カード番号の入力欄はありません。決済credentialはAppleが取り扱い、本アプリのbackendには提供されません。

AI Tutorや誤植報告の自由入力欄は、入力した内容をそのまま取り扱います。氏名、Firebase UID、メールアドレス、認証情報、その他の個人情報や機微な情報を入力しないでください。現行backendは、これらの自由入力を自動で検出、削除または匿名化しません。

RevenueCat SDKは、自動device identifier収集とdiagnosticsを無効化し、native logをapplication logへ転送しない設定で初期化します。氏名、メールアドレス、IDFA、IDFV、広告またはattribution用のcustomer属性を独自に設定しません。

## 第三者サービス

| サービス | 用途 |
|---|---|
| Firebase Authentication | 匿名認証、Sign in with Apple連携、ID token発行、利用者削除 |
| Sign in with Apple | 購入前の本人確認、アカウント復旧、削除時の再認証とtoken失効 |
| Google Cloud Run / Cloud SQL | backend API、監査ログ、rate limit、誤植報告、課金情報、削除処理台帳の保存と処理 |
| LLM provider API | AI Tutorの回答生成。質問文、学習状態、問題本文、正誤を含む選択肢、解説、出典情報を送信する |
| RevenueCat | App Store購入の検証、購入状態と利用権限の管理、復元、返金、customer削除 |
| Google Forms / Gmail | betaフィードバック、自己完結削除を持たない旧TestFlight buildの削除依頼、特定商取引法上の住所・電話番号の開示請求と返信 |
| TestFlight / App Store Connect | beta配布、App Store配布、購入と返金の処理 |

LLM provider API key、database接続情報、backend secretはモバイルアプリに含めません。第三者の取扱いは各提供者の規約とポリシーにも従います。[Firebaseのプライバシー情報](https://firebase.google.com/support/privacy/)、[RevenueCat Privacy Policy](https://www.revenuecat.com/privacy/)も確認してください。

## 保存期間

| 情報 | 起算点 | 保持期限後の処理 |
|---|---|---|
| AI Tutorの質問文、回答、引用、参照情報 | 監査recordの作成日時 | 180日後の次の日次処理で該当項目をNULLにする |
| AI Tutorのrequest ID、利用者との紐付け、mode、status、provider、model、prompt template version、guardrail結果、latency、token usage、plan、購入環境 | 監査recordの作成日時 | 13か月後の次の日次処理でrecord全体を削除する |
| AI Tutorの日次利用カウンター | UTCの利用日 | 31日後の次の日次処理でrecord全体を削除する |
| backendへ送信した誤植報告 | backendの受信日時 | 180日後の次の日次処理でrecord全体を削除する |
| Google Formsのbetaフィードバックと削除依頼、送信済みの削除完了メール | 送信日時 | 180日後に削除する |
| 特定商取引法の開示請求フォーム回答と返信メール | フォームまたはメールの送信日時 | 180日以内に削除する |
| 削除処理台帳と識別子のhash tombstone | 未開始は作成日時、完了は完了または完了連絡の記録日時 | 181日後の次の日次処理で削除する。未完了の`processing`または`failed`はrecordは復旧完了まで保持する |
| 自己完結削除のraw receipt | 発行日時 | 30日で失効し、完了または失効の確認後に端末から削除する |
| RevenueCat mapping、backendの課金record、RevenueCat customerと購入履歴 | アカウントと取引の存続中 | 購入確認と復元に必要な範囲で保持する。アカウント削除時はRevenueCat customerの削除確認後に関連mappingと利用者に紐付くrecordを削除する。法令上必要な取引記録とbackupには各提供者の保持規則が適用される |
| production Cloud SQLのbackup | backupの作成日時 | 自動backupは新しい7世代を保持し、PITR用transaction logは7日保持する。migration前のオンデマンドbackupは作成から30日以内に運用者が削除する |

端末内の回答履歴、復習予定、利用event、未送信の誤植報告は、アカウント削除の完了後にアプリが削除します。アカウント削除を行わない場合は、利用者がアプリを削除するまで端末内に残ります。backup内の削除済みrecordは個別に選択削除できません。backupをrestoreする場合は、利用再開前に同じ基準日時で保持処理を再実行します。Firebase Authenticationではアカウント削除開始後も、認証データが稼働系とbackupから削除されるまで最大180日かかる場合があります。

## アカウント削除

現行buildでは、アプリ内の「アカウントを削除」から開始できます。匿名利用者は確認画面から、Apple連携済み利用者は同じApple Accountで再認証してから開始します。Apple連携済みの場合はApple tokenを失効し、RevenueCat customer、backendの利用者データ、Firebase Authenticationの利用者を削除します。完了後は端末内の関連データも削除します。

アカウント削除は、App Storeのサブスクリプション解約、Apple Accountの削除、Appleが保持する取引記録の削除ではありません。有効な月額契約は、Apple Accountのサブスクリプション管理画面から別に解約してください。

## 共有、販売、トラッキング

取り扱う情報を販売しません。法令上必要な場合、サービス運用に必要な委託先、または不正利用調査に必要な範囲を除き、第三者へ提供しません。他社のアプリやWebサイトを跨いだ広告目的のトラッキングを行いません。

## App Storeのプライバシー申告

App Store Connectでは、アプリ本体と第三者SDKを合わせ、次の6種類を申告します。すべて「トラッキングに使用しない」です。自由入力やbackendの記録が認証済み利用者に紐付く可能性を含め、全6種類を「ユーザーに紐付ける」とします。

| App Storeの種類 | 主な対象 | 利用目的 |
|---|---|---|
| その他のユーザーコンテンツ | AI Tutorの質問、誤植報告の任意コメント | Appの機能、アナリティクス |
| ユーザーID | Firebase UID、backend内部ID、不透明なRevenueCat App User ID | Appの機能 |
| 購入履歴 | App Storeの購入、更新、復元、返金、失効 | Appの機能、アナリティクス |
| その他の使用状況データ | AI Tutorの利用回数、モード、認可時点のplanと購入環境 | Appの機能、アナリティクス |
| その他の診断データ | request status、guardrail結果、latency、token usage、SDKの診断metadata | Appの機能、アナリティクス |
| その他のデータの種類 | 認証やセキュリティで処理するIPアドレス、user agent等 | Appの機能、アナリティクス |

## 利用者の選択と問い合わせ

Sign in with Appleを行わずに、無料の問題演習、回答履歴、復習キューを利用できます。AI Tutorと誤植報告の利用は任意です。

一般的な問い合わせと不具合報告は、[公開サポートページ](https://shunki-235.github.io/cpa-short-answer-support/)から連絡してください。公開Issueには、Firebase UID、ID token、削除receipt、Apple Account情報、メールアドレスなどの個人情報、識別情報、認証情報を書き込まないでください。アカウント削除はアプリ内の専用導線を利用してください。

販売事業者の住所・電話番号の開示請求は、[特定商取引法に基づく表示](../commercial-transactions/)から専用フォームを利用してください。フォームは返信先メールアドレスだけを必須とし、GoogleへのログインやGoogle Accountの共有を必須にしません。回答はGoogle Formsのresponse storeだけに保存し、Google Sheetsへ連携しません。
