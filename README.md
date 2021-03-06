# Donation Tips

CasperLabs Signerによる、transferを行うことができます。

## Testnetでの実行方法

testnet.cspr.liveの[connected peers](https://testnet.cspr.live/tools/peers)より、ノードアドレスを取得します。

src/setupProxy.jsのtarget部分を書き換え、ポート番号は7777に設定します。

```setupProxy.js
app.use(
    '/testnet',
    createProxyMiddleware({
        target: 'http://159.65.118.250:7777/rpc',
        changeOrigin: true,
    })
);
```

次に、src/SignerTest.jsの、下記部分は'/testnet'に設定します。

```SignerTest.js
this.casperService = new CasperClient('/testnet')
```

また、networkNameは"casper-test"に設定します。

```SignerTest.js
let networkName = "casper-test";
```

以下のコマンドで、Reactフロントエンドアプリケーションを起動します。

```bash
$ yarn install
$ yarn start
```
senderがCSPR(Testnet)を保有していることを確認し、実行を行います。

動作確認を行うには、表示されたDeploy Hashを、[testnet.cspr.live](https://testnet.cspr.live/)で入力して確認します。

## NCTLでの実行方法

src/setupProxy.jsのtarget部分を、ターゲットノードアドレスに変更します。

```setupProxy.js
app.use(
    '/nctl',
    createProxyMiddleware({
        target: 'http://localhost:11101/rpc',
        changeOrigin: true,
    })
);
```

次に、src/SignerTest.jsの、下記部分は'/nctl'に設定します。

```SignerTest.js
this.casperService = new CasperClient('/nctl')
```

また、networkNameは"casper-net-1"に設定します。

```SignerTest.js
let networkName = "casper-net-1";
```

次に、faucetアカウントのsecret_keyを、Signerにインポートします。

```bash
$ cat /casper-node/utils/nctl/assets/net-1/faucet/secret_key.pem
```

送信先(user-1など)のpublic_key_hexを確認します。

```bash
$ cat /casper-node/utils/nctl/assets/net-1/users/user-1/public_key_hex
```

以下のコマンドで、Reactフロントエンドアプリケーションを起動します。

```bash
$ yarn install
$ yarn start
```

senderをfaucetアカウントとし、受取人をuser-1に指定して実行を行います。

動作確認を行うには、表示されたDeploy Hashを使用して、デプロイ情報の取得を行います。

``` bash
$ casper-client get-deploy --node-address http://localhost:11101 $DEPLOY_HASH
```

## ホワイトリスト

Casper Labs Signerをlocalhost以外で使用するには、Casper Labsの認可を受ける必要があります。

https://github.com/casper-ecosystem/signer/blob/master/public/manifest.json#L22