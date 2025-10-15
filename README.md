Claro! Vou te dar um exemplo bem simples de um arquivo único (HTML) que usa JavaScript para agendar uma notificação com base em uma data e horário que o usuário escolher. A notificação será disparada quando o horário chegar.

### Exemplo de Código

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Notificação Programada</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 20px;
        }

        .container {
            max-width: 400px;
            margin: 0 auto;
            text-align: center;
        }

        input[type="datetime-local"] {
            padding: 10px;
            font-size: 16px;
            margin-top: 10px;
            width: 100%;
        }

        button {
            padding: 10px 20px;
            font-size: 16px;
            margin-top: 20px;
            cursor: pointer;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
        }

        button:hover {
            background-color: #45a049;
        }

        .message {
            margin-top: 20px;
            font-size: 18px;
            color: #333;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Agendar Notificação</h1>
        <p>Escolha o horário para a notificação:</p>
        <input type="datetime-local" id="datetime">
        <button onclick="agendarNotificacao()">Agendar Notificação</button>
        <div class="message" id="message"></div>
    </div>

    <script>
        function agendarNotificacao() {
            const datetimeInput = document.getElementById('datetime').value;
            const messageDiv = document.getElementById('message');
            
            if (!datetimeInput) {
                messageDiv.textContent = 'Por favor, escolha uma data e hora.';
                return;
            }

            const dataEscolhida = new Date(datetimeInput);
            const agora = new Date();
            const tempoEspera = dataEscolhida - agora;

            if (tempoEspera <= 0) {
                messageDiv.textContent = 'O horário escolhido já passou. Escolha um horário no futuro.';
                return;
            }

            // Exibe mensagem de sucesso
            messageDiv.textContent = `Notificação agendada para ${dataEscolhida.toLocaleString()}`;

            // Agendar a notificação
            setTimeout(() => {
                if (Notification.permission === 'granted') {
                    new Notification('Hora da sua notificação!', {
                        body: 'Este é o momento que você agendou.',
                        icon: 'https://via.placeholder.com/100'
                    });
                } else {
                    // Solicitar permissão de notificação se necessário
                    Notification.requestPermission().then(permission => {
                        if (permission === 'granted') {
                            new Notification('Hora da sua notificação!', {
                                body: 'Este é o momento que você agendou.',
                                icon: 'https://via.placeholder.com/100'
                            });
                        }
                    });
                }
            }, tempoEspera);
        }
    </script>

</body>
</html>
```

### Explicação:

1. **HTML**:

   * A estrutura básica da página contém um título, um campo de entrada (`input`) para o usuário escolher a data e hora, e um botão para agendar a notificação.
   * Também há uma área para mostrar mensagens ao usuário, como o sucesso ou erro ao agendar a notificação.

2. **CSS**:

   * Um estilo simples para centralizar o conteúdo e deixar a interface mais amigável.

3. **JavaScript**:

   * A função `agendarNotificacao()` é chamada quando o usuário clica no botão. Ela pega a data e hora selecionadas no `input` e calcula a diferença entre o horário escolhido e o horário atual.
   * Se o horário escolhido for válido (no futuro), o `setTimeout` é usado para agendar a notificação.
   * O `Notification.requestPermission()` é usado para solicitar permissão ao navegador para mostrar notificações, caso o usuário ainda não tenha autorizado.

### Como funciona:

1. O usuário escolhe a data e a hora no campo de entrada.
2. Ao clicar no botão, o script verifica se o horário escolhido é válido (se está no futuro) e agendará a notificação.
3. Quando a hora chegar, a notificação será exibida. Se a permissão de notificação ainda não foi dada, o navegador pedirá ao usuário para permitir notificações.

### Testando:

* **Permissões de Notificação**: Ao carregar a página, o navegador pode pedir permissão para enviar notificações. O usuário precisa permitir isso para que a notificação seja exibida.

Esse é um exemplo simples, mas pode ser expandido conforme suas necessidades.
