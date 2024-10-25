Atividade requisitada pela disciplina de sistemas embarcados do programa PPGEC (Professor José Paulo G. de Oliveira)

Sistema Cliente-Servidor em Comunicação UART

Objetivo: Implementar um sistema de comunicação cliente-servidor utilizando o PC como cliente
e a placa NUCLEO-H753ZI como servidor. O cliente enviará comandos ao servidor (NUCLEO), que
responderá com o valor de um contador, controlará LEDs ou enviará uma tabela de informações
dos membros da equipe, dependendo do comando recebido.

ATENÇÃO: a interface que deve ser usada para conexão da NUCLEO com o computador é
USART3!!!

Descrição do Projeto:
1. Cliente (PC – terminal virtual):
– Comando por Software: o cliente (PC) enviará comandos pela UART, como
"Send_Count", "Clear_Count", "Table", "Led_R", "Led_G" ou "Led_B".
– Recepção de Dados: O cliente receberá do servidor o valor do contador, a tabela de
membros ou confirmação da mudança de estado dos LEDs.
2. Servidor (NUCLEO-H753ZI):
– Botão: ao pressionar o botão na placa (gerando uma interrupção EXTI), o valor de um
contador será incrementado.
– Comandos Recebidos: o servidor receberá os seguintes comandos via UART:
o "Send_Count": O servidor enviará o valor atual do contador ao cliente.
o "Clear_Count": O servidor zerará o contador.
o "Table": O servidor enviará uma tabela com os nomes e matrículas dos membros da
equipe.
o "Led_R", "Led_G", "Led_B": O servidor alternará o estado (ligado/desligado) dos
respectivos LEDs (R, G ou B).

3. Transmissão via DMA:
– A tabela com os nomes e matrículas dos membros será enviada ao cliente utilizando
DMA. Essa tabela também será exibida no terminal virtual do PC.

Etapas do Projeto:
1. Configuração da UART (NUCLEO-H753ZI):
o Configure a UART para comunicação serial com o PC (USART3).
o Defina os parâmetros adequados: baud rate (ex: 115200), 8 bits de dados, sem
paridade e 1 stop bit.
o Ative o suporte a DMA para recepção da tabela.

Sistemas Embarcados 2024.2
UPE Ecomp-PPGEC
José Paulo G. de Oliveira

2. Configuração do Servidor (NUCLEO-H753ZI):
o Botão (EXTI): Configure uma interrupção externa (EXTI) para detectar o
pressionamento do botão e incrementar o contador.
o Recepção de Comandos (UART): Utilize a função HAL_UART_Receive_IT() para
escutar os comandos enviados pelo PC.
o Tratamento de Comandos:
▪ "Send_Count": Ao receber este comando, o servidor deve enviar o valor
do contador para o cliente usando HAL_UART_Transmit_IT ().
▪ "Clear_Count": Ao receber este comando, o contador deve ser zerado.
▪ "Table": Ao receber este comando, envie a tabela de nomes e
matrículas (ex: char tabela[NUM_MEMBROS][30] = { "Nome1,
Matricula1", "Nome2, Matricula2", ... }) via UART.
▪ "Led_R", "Led_G", "Led_Y": Ao receber um desses comandos, alterne
o estado do respectivo LED.

o Controle dos LEDs: Configure as saídas para controlar os LEDs vermelho, verde
e azul da placa.

3. Comandos Enviados pelo Cliente (PC):
o Desenvolva um software (ou use um terminal serial) no PC para enviar os
seguintes comandos ao servidor:
▪ "Send_Count": Solicita o valor do contador.
▪ "Clear_Count": Solicita que o contador seja zerado.
▪ "Table": Solicita a tabela com os nomes e matrículas.
▪ "Led_R", "Led_G", "Led_B": Alterna o estado dos LEDs (vermelho,
verde e azul).

Critérios de Avaliação:

• Funcionamento correto da comunicação entre o cliente (PC) e o servidor (NUCLEO-
H753ZI).

• Implementação correta do incremento e envio do valor do contador.
• Implementação da lógica de zerar o contador e de alternar os LEDs conforme os
comandos.
• Envio correto da tabela de membros da equipe.
• Mudança de estado dos LEDs conforme os comandos "Led_R", "Led_G" ou "Led_B".
Recursos Necessários:
• Placa NUCLEO-H753ZI.
• Cabo USB para conexão da placa ao PC.

Sistemas Embarcados 2024.2
UPE Ecomp-PPGEC
José Paulo G. de Oliveira
• Software de terminal ou aplicação de cliente para envio de comandos (ex.: PuTTY,
Advanced Serial Terminal-Eltima, etc.).
• Ambiente de desenvolvimento STM32CubeIDE.
