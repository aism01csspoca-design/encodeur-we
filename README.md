<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title>Encodeur/Décodeur Réduction+Indice</title>
    <style>
        body { font-family: Arial, sans-serif; padding: 20px; }
        input, textarea { width: 100%; padding: 5px; margin: 5px 0; }
        button { padding: 10px 20px; margin: 5px; }
    </style>
</head>
<body>
    <h2>Encodeur/Décodeur Réduction + Indice</h2>

    <label>Entrez votre phrase :</label>
    <input type="text" id="inputText">

    <button onclick="coder()">Coder</button>
    <button onclick="decoder()">Décoder</button>

    <label>Résultat :</label>
    <textarea id="output" rows="4" readonly></textarea>

    <script>
        // Table des lettres par réduction
        const reductionTable = {
            1: ['A','J','S'],
            2: ['B','K','T'],
            3: ['C','L','U'],
            4: ['D','M','V'],
            5: ['E','N','W'],
            6: ['F','O','X'],
            7: ['G','P','Y'],
            8: ['H','Q','Z'],
            9: ['I','R']
        };

        function removeAccents(text) {
            return text.normalize("NFD").replace(/[\u0300-\u036f]/g, "");
        }

        function coderLettre(letter) {
            letter = letter.toUpperCase();
            for (let red in reductionTable) {
                let idx = reductionTable[red].indexOf(letter);
                if (idx !== -1) return red + (idx + 1);
            }
            return letter; // garde espaces ou ponctuation
        }

        function coder() {
            let text = removeAccents(document.getElementById("inputText").value);
            let coded = Array.from(text).map(c => coderLettre(c));
            document.getElementById("output").value = coded.join("-");
        }

        function decoderCode(code) {
            if (!/^\d\d$/.test(code)) return code; // espace ou ponctuation
            let red = parseInt(code[0]);
            let idx = parseInt(code[1]) - 1;
            if (reductionTable[red] && reductionTable[red][idx]) return reductionTable[red][idx];
            return '?';
        }

        function decoder() {
            let codes = document.getElementById("inputText").value.split("-");
            let decoded = codes.map(c => decoderCode(c)).join("");
            document.getElementById("output").value = decoded;
        }
    </script>
</body>
</html>

