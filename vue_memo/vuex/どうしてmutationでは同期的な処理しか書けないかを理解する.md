# どうしてmutationでは同期的な処理しか書けないかを理解する
- stateはDBに保存するものなので、非同期処理のようにB, A, Cとした場合ログを確認しずらい
- そのため、mutationsを間接的に変更するactionsで予測可能にする

[参考](https://zenn.dev/oreilly_ota/articles/4b6f0bdb8ba15d)
