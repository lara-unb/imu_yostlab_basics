# Tutorial básico do IMU


## Objetivos


* Compreender como ligar e utilizar as IMUs usando o Dongle conectado ao seu computador. 
* Entender o funcionamento geral do código e como mudá-lo para atender suas necessidades.


## Requisitos de Hardware


* Um computador com portas USB 
* 3-Space Kit (com o Dongle e IMUs) da Yostlabs


## Requisitos de software


* Python 3.6 ou superior. 
* Sistemas operacionais: Windows ou Linux


## Instalação e configuração


Recomenda-se o uso de um ambiente virtual (*venv*) para evitar erros de compatibilidade entre bibliotecas pré-instaladas.


1. ### Clonar o repositório


Clone o repositório dentro da pasta desejada: 
git clone \<url\>


2. ### Criar e ativar ambiente virtual


Entre na pasta do repositório: 
cd imu\_yostlab\_basics 
Crie o ambiente virtual *venv:* 
python3 \-m venv .venv 
E ative com o comando: 
source .venv/bin/activate \# Linux ou 
.venv\\Scripts\\activate.bat \# Windows


3. ### Instalar pacotes


Ainda dentro da pasta, instale os pacotes usando os seguintes comandos: 


```python
pip install matplotlib
pip install mediapipe 
pip install PyQt5
pip install pyserial
pip install pyqtgraph
pip install pyquaternion
pip install scipy
```


## Quick Start


### 1\. Conectando os Dongles e IMUs


Conecte o Dongle em uma porta USB. Verifique se o IMU está carregado e ligue-o segurando o botão até o LED brilhar. Para carregar, deixe o sensor conectado a porta USB do computador.


### 2\. Abra o código imu\_basic


### 3\. Altere o ID da IMU


ID é o número de identificação do sensor IMU. Este número estará etiquetado na própria IMU ou pode ser conferido através do aplicativo 3-Space Suit. Para isso baixe-o no site da Yostlabs ([https://yostlabs.com/yost-labs-3-space-sensor-software-suite/](https://yostlabs.com/yost-labs-3-space-sensor-software-suite/)), conecte o Dongle, selecione no campo de ID das IMUs cada número, até que o modelo do aplicativo responda a movimentação da sua IMU.


Para alterar o ID no código, altere a lista *imus* no início do código com o ID do IMU que você irá utilizar. 
*Exemplo:* 
*imus \= \[9\]*


### 4\. Altere as configurações da IMU


Na função *configure\_imu* altere os parâmetros conforme a sua necessidade:


* disableCompass: desabilita o magnetômetro; 
* disableGyro: desabilita o giroscópio; 
* disableAccelerometer: desabilita o acelerômetro; 
* gyroAutoCalib: calibra o giroscópio automaticamente. Caso este parâmetro esteja ativado, ao iniciar o código, deixe o sensor completamente parado; 
* tareSensor: tara orientação do sensor usando o quaternion atual dele. Caso este parâmetro esteja ativado, ao iniciar o código, deixe o sensor virado para cima e na sua direção em uma superfície reta; 
* tareWithQuaternion: caso a função tareSensor não esteja ativada, pode utilizar um quaternion desejado para tarar a orientação do sensor. A entrada é um dicionário, cuja chave é ‘imu’+’id’ (exemplo, ‘imu8’) e valor é uma lista com os valores das coordenadas do quaternion; 
* filterMode: 0 \- Sem filtro de orientação; 1 \- Filtro de Kalman; 2 \- Filtro Q-COMP; 3 \- Filtro Q-GRAD. Para mais informações sobre os modos de filtro, confira a página 42 do User’s Manual. Por padrão, o valor configurado é 1; 
* logical\_ids: insira uma lista com os IDs das IMUs utilizadas. Conforme o passo 3, basta inserir a variável imu\_ids; 
* streaming\_commands: de forma simplificada, recebe uma lista de 8 números inteiros de 0 a 255\. Estes números servem para que a IMU possa enviar um tipo determinado de informação. Cada diferente número equivale a um tipo de informação e isso pode ser conferido na página 44 do User’s Manual. 
* baudrate: baudrate, taxa de transmissão, é a velocidade da comunicação entre o computador e o Dongle/Sensor, medida em bits por segundo (bps). Por padrão, o valor está configurado como 115200, tal qual recomenda o manual. 
* show\_quaternion: Ao iniciar o streaming de dados, envia quaternions do IMU; 
* show\_euler\_angle: Ao iniciar o streaming de dados, envia ângulos de euler do IMU; 
* show\_rotation\_matrix: Ao iniciar o streaming de dados, envia matriz de rotação do IMU; 
* show\_accel: Ao iniciar o streaming de dados, envia as informações do acelerômetro do IMU; 
* show\_gyro: Ao iniciar o streaming de dados, envia as informações do giroscópio do IMU; 
* show\_button: Ao iniciar o streaming de dados, envia as informações do botão do IMU; 
* show\_compass: Ao iniciar o streaming de dados, envia as informações do magnetômetro do IMU;


Por padrão, a função só possui os parâmetros *disableCompass*, *gyroAutoCalib*, *show\_quaternion*, *show\_euler\_angle* configurados como *True* por padrão. Além disso, a função retorna uma lista com os códigos e posições dos tipos de informação solicitados para o streaming, que será necessário para extrair as informações daquele IMU com a função *extract\_data*.


### 5\. Altere os parâmetros do streaming


* *duration*: duração do streaming. Por padrão, está configurado como tempo indefinido. 
* *delay*: tempo de atraso em —segundos para começar o streaming 
* *frequency:* frequência a qual o streaming enviará as informações, podendo configurar no máximo, uma frequência de 1000 Hz ou, caso opte por deixar a configuração padrão (0), o quão rápido for possível fazer o envio. 
 * Obs: a frequência máxima depende do *filterMode* configurado: 
   * Sem filtro de orientação (0): até 1350 Hz 
   * Filtro de Kalman (1): Até 250 Hz 
   * Q-COMP e Q-GRAD (2 e 3): até 350 Hz


### 6\. Receber e extrair os dados da IMU.

Antes de começar a ler os dados, o código utiliza a função serial_port.reset_input_buffer(), para limpar o buffer. 

A função read_data recebe as informações enviadas pelo IMU. Já a função extract_data é utilizada para  extrair informações de acelerômetro, quaternion, ângulo de euler e etc. Além disso, também inicializa-se a variável, que será usada para armazenar a informação, como None para verificar se a extração para o IMU requisitado no parâmetro da função foi bem sucedida. Então você salva o valor em outra variável e a atribui novamente para None, como no exemplo abaixo:


```python
current_accel = None 
   while True:
       try:
       data = imu_yostlabs_lara.read_data(serial_port)


       if data is not None:
           accel \= imu\_yostlabs\_lara.extract\_data(data, type\_of\_data \= 39, imu\_id \= imus, streaming\_slots=streaming\_slots1)


           if accel is not None:
               current_accel = accel


       if (current_accel is not None):
           current_accel = None
```


No parâmetro *type\_of\_data*, selecione qual tipo de informação você quer extrair, conforme a lista abaixo: 
&emsp;0(0x00) \- Quaternion 
&emsp;1(0x01) \- Ângulo de Euler 
&emsp;2(0x02) \- Matriz de rotação 
&emsp;38(0x26) \- Giroscópio 
&emsp;39(0x27) \- Acelerômetro 
&emsp;40(0x28) \- Magnetômetro 
&emsp;250(0xfa) \- Estado do botão 
Lembre-se de que você deve ter configurado no passo 4 para mostrar o tipo de informação desejada, caso contrário, não será possível realizar a extração.


### 7\. Execute o código (e faça a calibração)
Antes de executar o código, caso esteja no Linux, rode o seguinte comando para conceder permissão a porta USB:

```python
	sudo chmod 666 /dev/ttyACM0
```

Quando o código estiver em execução, antes de apertar Enter para iniciar a configuração, deixe o IMU virado para cima e em sua direção em uma superfície plana e reta para que ele faça a calibração e a tara corretamente.

# [Documentação](https://github.com/lara-unb/imu_yostlab_basics/blob/main/DOCUMENTACAO.md)
