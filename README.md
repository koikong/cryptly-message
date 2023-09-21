<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>メッセージ暗号化/復号化プログラム</title>
</head>
<body>
    <h2>Commands:</h2>
    <p>1. <strong>encrypt/message/password:</strong> メッセージを暗号化します。</p>
    <p>2. <strong>decrypt/encryptedMessage/password:</strong> 暗号文を復号化します。</p>
    <p>3. <strong>help:</strong> ヘルプを表示します。</p>
    <p>4. <strong>exit:</strong> プログラムを終了します。</p>

    <form id="commandForm">
        <label for="commandInput">コマンドを入力してください:</label>
        <input type="text" id="commandInput" required>
        <button type="button" id="submitButton">実行</button>
    </form>

    <div id="outputArea"></div>

    <script>
        // (前のスクリプトと同じ)

        function main() {
            const commandForm = document.getElementById("commandForm");
            const commandInput = document.getElementById("commandInput");
            const submitButton = document.getElementById("submitButton");
            const outputArea = document.getElementById("outputArea");

            submitButton.addEventListener("click", function () {
                const command = commandInput.value.trim();

                if (command === "help") {
                    outputArea.textContent = printHelp();
                } else if (command === "exit") {
                    // プログラムを終了する処理を追加
                    outputArea.textContent = "プログラムを終了しました。";
                } else if (command.startsWith("encrypt/")) {
                    const args = command.split("/");
                    if (args.length === 3) {
                        const message = args[1];
                        const password = args[2];
                        const encryptedValue = encryptMessage(message, password);
                        outputArea.textContent = "暗号化された値: " + JSON.stringify(encryptedValue);
                        // 暗号文をクリップボードにコピー
                        copyToClipboard(JSON.stringify(encryptedValue));
                    } else {
                        outputArea.textContent = "コマンドの引数が不正です。";
                    }
                } else if (command.startsWith("decrypt/")) {
                    const args = command.split("/");
                    if (args.length === 3) {
                        const encryptedValue = JSON.parse(args[1]);
                        const password = args[2];
                        const decryptedMessage = decryptMessage(encryptedValue, password);
                        outputArea.textContent = "復号化されたメッセージ: " + decryptedMessage;
                    } else {
                        outputArea.textContent = "コマンドの引数が不正です。";
                    }
                } else {
                    outputArea.textContent = "無効なコマンドです。";
                }
            });
        }

        main();
    </script>
</body>
</html>
