# eletromec-ia
IA de manutenção eletromecânica - Águas de Manaus
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EletroMec IA - Águas de Manaus</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: Arial, sans-serif;
            background: #1a1a2e;
            color: #fff;
            min-height: 100vh;
        }
        .header {
            background: #16213e;
            padding: 15px;
            text-align: center;
            border-bottom: 3px solid #e94560;
        }
        .header h1 {
            font-size: 1.5em;
            color: #e94560;
        }
        .header p {
            font-size: 0.9em;
            color: #a0a0a0;
            margin-top: 5px;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 15px;
        }
        .menu {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }
        .menu-btn {
            background: #16213e;
            border: 2px solid #e94560;
            color: #fff;
            padding: 15px;
            border-radius: 10px;
            font-size: 1em;
            cursor: pointer;
            transition: all 0.3s;
        }
        .menu-btn:hover {
            background: #e94560;
            transform: scale(1.05);
        }
        .menu-btn.active {
            background: #e94560;
        }
        .chat-area {
            background: #16213e;
            border-radius: 10px;
            padding: 15px;
            height: 400px;
            overflow-y: auto;
            margin-bottom: 15px;
        }
        .message {
            margin-bottom: 15px;
            padding: 10px;
            border-radius: 10px;
            max-width: 85%;
        }
        .user {
            background: #0f3460;
            margin-left: auto;
            text-align: right;
        }
        .ai {
            background: #533483;
        }
        .message .time {
            font-size: 0.7em;
            color: #a0a0a0;
            margin-top: 5px;
        }
        .input-area {
            display: flex;
            gap: 10px;
        }
        .input-area input {
            flex: 1;
            padding: 12px;
            border: 2px solid #e94560;
            border-radius: 10px;
            background: #16213e;
            color: #fff;
            font-size: 1em;
        }
        .input-area button {
            padding: 12px 20px;
            background: #e94560;
            border: none;
            border-radius: 10px;
            color: #fff;
            font-size: 1em;
            cursor: pointer;
        }
        .reminders {
            background: #16213e;
            border-radius: 10px;
            padding: 15px;
            margin-top: 20px;
        }
        .reminders h3 {
            color: #e94560;
            margin-bottom: 10px;
        }
        .reminder-item {
            background: #0f3460;
            padding: 10px;
            border-radius: 8px;
            margin-bottom: 10px;
        }
        .reminder-item .time {
            color: #e94560;
            font-weight: bold;
        }
        .reminder-item .task {
            color: #fff;
            margin-top: 5px;
        }
        .hidden {
            display: none;
        }
        @media (max-width: 600px) {
            .menu {
                grid-template-columns: 1fr 1fr;
            }
            .chat-area {
                height: 350px;
            }
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>⚡ EletroMec IA</h1>
        <p>Águas de Manaus - Manutenção Eletromecânica</p>
    </div>

    <div class="container">
        <div class="menu">
            <button class="menu-btn active" onclick="showChat()">💬 Chat</button>
            <button class="menu-btn" onclick="showReminders()">⏰ Lembretes</button>
        </div>

        <div id="chatSection">
            <div class="chat-area" id="chatArea">
                <div class="message ai">
                    <p>Olá! Sou a EletroMec IA, sua assistente de manutenção eletromecânica. Como posso ajudar hoje?</p>
                    <div class="time">Agora</div>
                </div>
            </div>
            <div class="input-area">
                <input type="text" id="userInput" placeholder="Digite sua dúvida..." onkeypress="if(event.key==='Enter')sendMessage()">
                <button onclick="sendMessage()">Enviar</button>
            </div>
        </div>

        <div id="remindersSection" class="reminders hidden">
            <h3>⏰ Lembretes da Semana</h3>
            <div class="reminder-item">
                <div class="time">Segunda - 8h</div>
                <div class="task">Separar atividades da equipe</div>
            </div>
            <div class="reminder-item">
                <div class="time">Terça - 8h</div>
                <div class="task">Verificar relatórios de manutenção</div>
            </div>
            <div class="reminder-item">
                <div class="time">Quarta - 8h</div>
                <div class="task">Inspeção visual nas subestações</div>
            </div>
            <div class="reminder-item">
                <div class="time">Quinta - 8h</div>
                <div class="task">Revisar sobreavisos da semana</div>
            </div>
            <div class="reminder-item">
                <div class="time">Sexta - 8h</div>
                <div class="task">Preparar relatório semanal</div>
            </div>
        </div>
    </div>

    <script>
        function showChat() {
            document.getElementById('chatSection').classList.remove('hidden');
            document.getElementById('remindersSection').classList.add('hidden');
            document.querySelectorAll('.menu-btn')[0].classList.add('active');
            document.querySelectorAll('.menu-btn')[1].classList.remove('active');
        }

        function showReminders() {
            document.getElementById('chatSection').classList.add('hidden');
            document.getElementById('remindersSection').classList.remove('hidden');
            document.querySelectorAll('.menu-btn')[1].classList.add('active');
            document.querySelectorAll('.menu-btn')[0].classList.remove('active');
        }

        function sendMessage() {
            const input = document.getElementById('userInput');
            const message = input.value.trim();
            if (!message) return;

            const chatArea = document.getElementById('chatArea');
            const now = new Date();
            const time = now.getHours() + ':' + String(now.getMinutes()).padStart(2, '0');

            // User message
            chatArea.innerHTML += '<div class="message user"><p>' + message + '</p><div class="time">' + time + '</div></div>';

            // AI response
            setTimeout(function() {
                const response = getAIResponse(message);
                chatArea.innerHTML += '<div class="message ai"><p>' + response + '</p><div class="time">' + time + '</div></div>';
                chatArea.scrollTop = chatArea.scrollHeight;
            }, 500);

            input.value = '';
            chatArea.scrollTop = chatArea.scrollHeight;
        }

        function getAIResponse(question) {
            question = question.toLowerCase();

            if (question.includes('motor') && question.includes('800')) {
                return 'Sobre o motor 800HP 6,6kV:\n\n✅ Motor testado - Isolação OK\n✅ Cabos do motor - OK\n✅ Seleção da bomba - OK\n❌ Cabos de saída do inversor - BAIXA ISOLAÇÃO\n\nCausa: Harmônicas do inversor Robicon degradando a isolação dos cabos 15kV.\n\nSolução: Substituir cabos por EPR 25kV + instalar filtro de saída.';
            }
            if (question.includes('bomba') && question.includes('submersa')) {
                return 'Diagnóstico de bomba submersa:\n\n1. Verificar alimentação no painel\n2. Testar isolação do motor\n3. Medir resistência entre fases\n4. Verificar selo mecânico\n5. Testar contator e relé\n\nQual o sintoma específico?';
            }
            if (question.includes('inversor') || question.includes('frequencia')) {
                return 'Diagnóstico de inversor de frequência:\n\n1. Verificar código de erro no display\n2. Medir tensão na entrada\n3. Verificar fusíveis e contatores\n4. Testar motor separado\n5. Verificar cabos de saída\n\nQual o erro que está aparecendo?';
            }
            if (question.includes('subestação') || question.includes('subestacao')) {
                return 'Checklist de subestação 13,8kV:\n\n1. Verificar nível de óleo dos transformadores\n2. Inspeção visual dos isoladores\n3. Medir temperatura dos conexões\n4. Verificar aterramento\n5. Testar proteções\n\nPrecisa de mais detalhes sobre algum item?';
            }
            if (question.includes('soft') && question.includes('starter')) {
                return 'Diagnóstico de soft-starter:\n\n1. Verificar tensão de alimentação\n2. Testar tiristores\n3. Verificar contator de bypass\n4. Testar motor separado\n5. Verificar programação\n\nQual o sintoma apresentado?';
            }
            if (question.includes('relatorio') || question.includes('relatório')) {
                return 'Modelo de relatório de manutenção:\n\nRELATÓRIO TÉCNICO\nData: [data]\nEquipamento: [equipamento]\n\n1. Descrição do problema\n2. Diagnóstico realizado\n3. Causa provável\n4. Ações corretivas\n5. Materiais utilizados\n6. Tempo de execução\n\nPrecisa de ajuda com algum relatório específico?';
            }
            if (question.includes('lembrete') || question.includes('agendamento')) {
                return 'Seus lembretes estão programados para às 8h:\n\n• Segunda: Separar atividades\n• Terça: Verificar relatórios\n• Quarta: Inspeção subestações\n• Quinta: Revisar sobreavisos\n• Sexta: Relatório semanal\n\nQuer adicionar algum lembrete novo?';
            }
            if (question.includes('fiação') || question.includes('fiacao') || question.includes('cabo')) {
                return 'Sobre fiação e cabos:\n\nPara dimensionamento de cabos:\n1. Calcular corrente do circuito\n2. Considerar queda de tensão (máx 3%)\n3. Verificar método de instalação\n4. Aplicar fator de correção\n\nPara cabos de média tensão com inversor:\n• Usar cabo EPR (15kV ou 25kV)\n• Respeitar comprimento máximo\n• Aterrar blindagem corretamente\n\nQual o caso específico?';
            }
            if (question.includes('energia') || question.includes('falta') || question.includes('queda')) {
                return 'Problemas de energia:\n\nVerificar:\n1. Qualidade da energia na entrada\n2. Quedas de tensão\n3. Desbalanceamento entre fases\n4. Harmônicas na rede\n5. Aterramento\n\nQual o problema específico que está ocorrendo?';
            }
            if (question.includes('diagnostico') || question.includes('diagnóstico')) {
                return 'Vou te ajudar no diagnóstico!\n\nMe diga:\n1. Qual equipamento? (motor, bomba, inversor, subestação)\n2. Qual o sintoma? (não liga, desarma, vibra, esquenta)\n3. Há quanto tempo ocorre?\n4. Já fez algum teste?\n\nCom essas informações, monto o passo a passo para você!';
            }
            if (question.includes('olá') || question.includes('ola') || question.includes('oi') || question.includes('bom dia') || question.includes('boa tarde') || question.includes('boa noite')) {
                return 'Olá! Sou a EletroMec IA da Águas de Manaus! ⚡\n\nPosso ajudar com:\n• Diagnóstico de equipamentos\n• Dimensionamento de cabos\n• Manutenção de motores e bombas\n• Subestações e inversores\n• Relatórios técnicos\n• Lembretes e agendamentos\n\nO que você precisa?';
            }
            if (question.includes('ajuda') || question.includes('help')) {
                return 'Como posso ajudar? Posso te auxiliar com:\n\n1. 🔧 Diagnóstico de equipamentos\n2. ⚡ Problemas elétricos\n3. 🌀 Bombas e motores\n4. 📋 Relatórios técnicos\n5. ⏰ Lembretes e agendamentos\n\nDigite sua dúvida ou escolha um assunto!';
            }

            return 'Entendi sua pergunta! Para te ajudar melhor, me diga:\n\n1. Qual equipamento está com problema?\n2. Quais os sintomas?\n3. Já fez algum teste?\n\nOu escolha um assunto: motor, bomba, inversor, subestação, fiação, energia, relatório.';
        }
    </script>
</body>
</html>
