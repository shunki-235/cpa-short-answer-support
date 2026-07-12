---
layout: page
title: CPA短答トレーナー TestFlightプライバシーポリシー
permalink: /privacy/
---

最終更新日: 2026-07-12

このプライバシーポリシーは、CPA短答トレーナーのTestFlight betaに適用されます。本アプリは公認会計士試験・短答式試験の学習支援を目的とした非公式アプリであり、試験実施機関または関係団体が提供、承認、監修する公式アプリではありません。

## 収集する情報

初回TestFlightでは、以下の情報を扱います。

| 情報 | 保存場所 | 用途 |
|---|---|---|
| Firebase anonymous UID | Firebase、backend | AI Tutorの認証、rate limit、濫用防止。TYPO_REPORT_API_URL有効buildでは誤植報告の送信者識別にも使い、backend上では誤植報告と同じ保持期間で扱う |
| AI Tutorへの質問文 | backend | 回答生成、品質確認、問い合わせ対応 |
| AI Tutorの回答、参照した問題ID、content version、監修状態 | backend | 監査、品質改善、誤回答調査 |
| AI Tutorのrequest status、latency、model、token usage | backend | 障害調査、費用監視、品質改善 |
| 回答履歴、復習予定、自信度、理由タグ | 端末内 | 復習キュー表示 |
| 誤植報告 | 端末内、TYPO_REPORT_API_URL有効buildではstaging backend | beta中の問題品質確認。対象箇所、対象テキスト、ユーザーコメントを扱う |

氏名、メールアドレス、電話番号、住所、位置情報、連絡先、写真、カレンダー、決済情報は初回TestFlightでは収集しません。実課金と購入復元も初回TestFlightには含みません。

## 利用目的

収集した情報は、以下の目的で利用します。

- AI Tutorへの質問に回答するため
- AI Tutorのrate limitと濫用防止を行うため
- 不具合、エラー、根拠不足回答、ガードレール違反を調査するため
- betaテスターからの問い合わせ、誤植報告に対応するため
- 問題演習、復習キュー、AI Tutor導線の品質を改善するため

## 第三者サービス

初回TestFlightでは、以下の第三者サービスを利用します。

| サービス | 用途 |
|---|---|
| Firebase Authentication | AI Tutor用の匿名認証 |
| Google Cloud Run / Cloud SQL | backend API、監査ログ、rate limit、TYPO_REPORT_API_URL有効buildの誤植報告保存 |
| LLM provider API | AI Tutor回答生成 |
| TestFlight / App Store Connect | beta配布 |

LLM provider API key、database接続情報、backend secretはモバイルアプリに含めません。

## 保存期間

AI Tutorの質問文、回答、参照コンテキストは、品質改善と問い合わせ対応のため最大180日保持し、その後削除または匿名化します。request status、latency、token usageなどの集計metadataとセキュリティ関連イベントは、運用監視と濫用調査のため最大13か月保持します。

端末内に保存される回答履歴、復習予定、未送信の誤植報告は、ユーザーがアプリを削除するか、後続機能で削除手段が追加されるまで端末内に残ります。TYPO_REPORT_API_URL有効buildでbackendへ送信された誤植報告と、それに紐付くFirebase anonymous UID由来のbackend内ユーザーIDは、問題品質確認と問い合わせ対応のため最大180日保持し、その後削除または匿名化します。

## 共有と販売

収集した情報を広告目的で販売または共有しません。法令上必要な場合、サービス運用に必要な委託先、または不正利用調査に必要な範囲を除き、第三者へ提供しません。

## ユーザーの選択

AI Tutorを使わずに、問題演習、回答履歴、復習キューを利用できます。AI Tutorを利用した場合、質問文と関連する監査ログがbackendに保存されます。誤植報告は任意で、TYPO_REPORT_API_URL有効buildでは送信した報告内容がstaging backendに保存されます。

## 問い合わせ

TestFlight betaに関する問い合わせ、不具合報告、削除依頼は[CPA短答トレーナー サポート](https://shunki-235.github.io/cpa-short-answer-support/)から連絡してください。公開Issueには個人情報や機微な情報を書き込まないでください。
