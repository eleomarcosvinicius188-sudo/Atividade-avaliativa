# Atividade-avaliativa


<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Consulta de Notas</title>
  <link rel="stylesheet" href="marcos.css">
    <script src="javas.js"defer></script>
</head>
<body>
    <main class="container">
        <section class="card">
            <h1>Sistema de Notas</h1>
            <p class="descricao">
                Consulte a média e a situação acadêmica do estudante.
            </p>

            <form id="formNotas">
                <label for="nome">Nome do estudante</label>
                <input
                    type="text"
                    id="nome"
                    placeholder="Digite o nome completo"
                >

                <div class="notas">
                    <div>
                        <label for="nota1">Nota 1</label>
                        <input type="number" id="nota1"
                            min="0" max="10" step="any"
                            placeholder="0 a 10">
                    </div>

                    <div>
                        <label for="nota2">Nota 2</label>
                        <input type="number" id="nota2"
                            min="0" max="10" step="any"
                            placeholder="0 a 10">
                    </div>

                    <div>
                        <label for="nota3">Nota 3</label>
                        <input type="number" id="nota3"
                            min="0" max="10" step="any"
                            placeholder="0 a 10">
                    </div>
                </div>

                <button type="submit" class="btn-calcular">
                    Calcular média
                </button>

                <button type="button" id="btnLimpar"
                    class="btn-limpar">
                    Reiniciar consulta
                </button>
            </form>

            <div id="resultado" class="resultado"
                role="status" aria-live="polite" hidden>
            </div>
        </section>

        <footer>
            <p>Sistema de Consulta de Notas | Desenvolvimento Web</p>
        </footer>
    </main>
</body>
</html>



* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg, #0f172a, #1e3a5f);
    color: #1e293b;
    min-height: 100vh;
    padding: 30px 16px;
}

.container {
    width: 100%;
    max-width: 650px;
    margin: 40px auto;
}

.card {
    background-color: #ffffff;
    padding: 32px;
    border-radius: 16px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.25);
}

h1 {
    text-align: center;
    color: #1e3a5f;
    margin-bottom: 12px;
}

.descricao {
    text-align: center;
    color: #64748b;
    line-height: 1.5;
    margin-bottom: 28px;
}

label {
    display: block;
    font-weight: bold;
    margin-bottom: 8px;
    color: #334155;
}

input {
    width: 100%;
    padding: 12px;
    border: 1px solid #cbd5e1;
    border-radius: 8px;
    font-size: 16px;
    margin-bottom: 20px;
    outline: none;
}

input:focus {
    border-color: #2563eb;
    box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.15);
}

.notas {
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 12px;
}

button {
    width: 100%;
    padding: 13px;
    border: none;
    border-radius: 8px;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
    transition: background-color 0.2s, transform 0.2s;
}

.btn-calcular {
    background-color: #2563eb;
    color: white;
    margin-bottom: 12px;
}

.btn-calcular:hover {
    background-color: #1d4ed8;
    transform: translateY(-2px);
}

.btn-limpar {
    background-color: #e2e8f0;
    color: #334155;
}

.btn-limpar:hover {
    background-color: #cbd5e1;
    transform: translateY(-2px);
}

.resultado {
    margin-top: 24px;
    padding: 20px;
    border-radius: 10px;
    background-color: #f1f5f9;
    text-align: center;
    line-height: 1.8;
    overflow-wrap: anywhere;
}

.resultado h2 {
    margin-bottom: 8px;
}

.resultado.aprovado {
    background-color: #dcfce7;
    color: #166534;
}

.resultado.recuperacao {
    background-color: #fef3c7;
    color: #92400e;
}

.resultado.reprovado {
    background-color: #fee2e2;
    color: #991b1b;
}

.resultado.erro {
    background-color: #fef2f2;
    color: #b91c1c;
}

footer {
    text-align: center;
    color: #cbd5e1;
    font-size: 13px;
    margin-top: 24px;
}

@media (max-width: 480px) {
    .container {
        margin: 10px auto;
    }

    .card {
        padding: 22px 16px;
    }

    .notas {
        grid-template-columns: 1fr;
        gap: 0;
    }
}



const formulario = document.getElementById("formNotas");
const resultado = document.getElementById("resultado");
const botaoLimpar = document.getElementById("btnLimpar");

formulario.addEventListener("submit", function(event) {
    event.preventDefault();

    const nome = document.getElementById("nome").value.trim();

    const valor1 = document.getElementById("nota1").value;
    const valor2 = document.getElementById("nota2").value;
    const valor3 = document.getElementById("nota3").value;

    // Verifica se o nome foi preenchido
    if (nome === "") {
        mostrarErro("Digite o nome do estudante.");
        return;
    }

    // Verifica se todas as notas foram preenchidas
    if (valor1 === "" || valor2 === "" || valor3 === "") {
        mostrarErro("Preencha as três notas.");
        return;
    }

    const nota1 = Number(valor1);
    const nota2 = Number(valor2);
    const nota3 = Number(valor3);

    // Verifica se as notas são válidas
    const notas = [nota1, nota2, nota3];

    const notasValidas = notas.every(function(nota) {
        return Number.isFinite(nota) &&
               nota >= 0 &&
               nota <= 10;
    });

    if (!notasValidas) {
        mostrarErro("As notas devem estar entre 0 e 10.");
        return;
    }

    // Calcula a média aritmética
    const media = (nota1 + nota2 + nota3) / 3;

    // Define a situação acadêmica
    let situacao;
    let classe;

    if (media >= 7) {
        situacao = "Aprovado(a)";
        classe = "aprovado";
    } else if (media >= 5) {
        situacao = "Recuperação";
        classe = "recuperacao";
    } else {
        situacao = "Reprovado(a)";
        classe = "reprovado";
    }

    // Exibe o resultado na página
    resultado.className = "resultado " + classe;
    resultado.innerHTML = `
        <h2>Resultado da consulta</h2>
        <p><strong>Estudante:</strong> ${escaparHTML(nome)}</p>
        <p><strong>Média:</strong> ${media.toFixed(2).replace(".", ",")}</p>
        <p><strong>Situação:</strong> ${situacao}</p>
    `;
    resultado.hidden = false;
});

// Exibe mensagens de erro
function mostrarErro(mensagem) {
    resultado.className = "resultado erro";
    resultado.textContent = mensagem;
    resultado.hidden = false;
}

// Evita interpretar o nome digitado como código HTML
function escaparHTML(texto) {
    return texto.replace(/[&<>"']/g, function(caractere) {
        const entidades = {
            "&": "&amp;",
            "<": "&lt;",
            ">": "&gt;",
            '"': "&quot;",
            "'": "&#39;"
        };

        return entidades[caractere];
    });
}

// Limpa todos os campos e o resultado
botaoLimpar.addEventListener("click", function() {
    formulario.reset();
    resultado.hidden = true;
    resultado.textContent = "";
    resultado.className = "resultado";
    document.getElementById("nome").focus();
});
