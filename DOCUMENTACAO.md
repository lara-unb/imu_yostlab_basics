\# Documentação





\### \*\*`initialize\_dongle(<imu\_ids>)`\*\*





Inicializa a conexão serial com o Dongle (receptor sem fio) conectado ao computador.





\* \*\*Parâmetros:\*\*`<imu\_ids>` (list): Lista com os IDs (números inteiros) das IMUs que se comunicarão com o Dongle. 

\* \*\*Retorno:\*\*`serial\_port`: Objeto de conexão serial configurado para comunicação.





\---





\### \*\*`initialize\_sensor(<imu\_ids>)`\*\*





Inicializa a conexão serial diretamente com um sensor IMU conectado via cabo USB ao computador.





\* \*\*Parâmetros:\*\*`<imu\_ids>` (list): Lista com o ID do sensor conectado. 

\* \*\*Retorno:\*\*`serial\_port`: Objeto de conexão serial configurado.





\---





\### \*\*`configure\_imu(<serial\_port>, <imu\_ids>, \[opcionais...])`\*\*





Configura os parâmetros internos do sensor (filtros, calibração, tara) e define quais pacotes de dados serão enviados durante o streaming.





\* \*\*Parâmetros:\*\*`<serial\_port>`: A porta serial ativa (Dongle ou Sensor). 

&#x20;\* `<imu\_ids>` (list): Lista de IDs lógicos das IMUs sendo configuradas. 

&#x20;\* `\[disableCompass]` (bool): Desabilita o magnetômetro (Padrão: `True`). 

&#x20;\* `\[disableGyro]` / `\[disableAccelerometer]` (bool): Desabilitam giroscópio e acelerômetro, respectivamente (Padrão: `False`). 

&#x20;\* `\[gyroAutoCalib]` (bool): Calibra o giroscópio automaticamente. O sensor deve estar estático (Padrão: `True`). 

&#x20;\* `\[tareSensor]` (bool): Tara a orientação do sensor usando o quaternion atual. Requer o sensor plano e apontado para você (Padrão: `True`). 

&#x20;\* `\[tareWithQuaternion]` (dict): Dicionário (ex: `{'imu8': \[w, x, y, z]}`) para tarar a orientação com um quaternion específico (Padrão: `None`). 

&#x20;\* `\[filterMode]` (int): Define o filtro de orientação (0: Sem filtro, 1: Kalman \[Padrão], 2: Q-COMP, 3: Q-GRAD). 

&#x20;\* `\[baudrate]` (int): Taxa de transmissão de dados (Padrão: `115200` bps). 

&#x20;\* \*\*Flags de Streaming (bool):\*\* `\[show\_quaternion]` (Padrão: `True`), `\[show\_euler\_angle]` (Padrão: `True`), `\[show\_accel]` (Padrão: `False`), `\[show\_gyro]` (Padrão: `False`), `\[show\_compass]` (Padrão: `False`), `\[show\_rotation\_matrix]` (Padrão: `False`), `\[show\_button]` (Padrão: `False`). Definem quais dados a IMU enviará. 

\* \*\*Retorno:\*\* `streaming\_commands` (list): Lista com os códigos dos dados solicitados.





\---





\### \*\*`start\_streaming(<serial\_port>, <imu\_ids>, <frequency>, \[timestamp], \[duration], \[delay])`\*\*





Inicia o envio contínuo de dados pelas IMUs configuradas de acordo com os parâmetros estabelecidos.





\* \*\*Parâmetros:\*\* `<serial\_port>`: A porta serial ativa. 

&#x20;\* `<imu\_ids>` (list): Lista de IDs das IMUs que iniciarão o streaming. 

&#x20;\* `<frequency>` (int): Frequência de envio de dados em Hz. Valor `0` envia o mais rápido possível. 

&#x20;\* `\[timestamp]` (bool): Inclui carimbo de tempo nos dados (Padrão: `False`). 

&#x20;\* `\[duration]` (int): Duração do streaming. Por padrão, ocorre por tempo indefinido. 

&#x20;\* `\[delay]` (int): Atraso em segundos antes de iniciar o fluxo de dados (Padrão: `0`).





\---





\### \*\*`read\_data(<serial\_port>)`\*\*





Lê as informações brutas enviadas pela IMU que estão aguardando no buffer da porta serial.





\* \*\*Parâmetros:\*\* `<serial\_port>`: A porta serial de onde os dados serão lidos. 

\* \*\*Retorno:\*\* data` (bytes): Os dados brutos capturados do sensor. Retorna `None` se não houver dados.





\---





\### \*\*`extract\_data(<data>, <type\_of\_data>, <imu\_id>, <streaming\_slots>, \[usb])`\*\*





Interpreta o pacote de bytes bruto recebido e extrai a informação matemática/física desejada.





\* \*\*Parâmetros:\*\* `<data>` (bytes): Os dados brutos retornados pela função `read\_data`. 

&#x20;\* `<type\_of\_data>` (int): O código do tipo de informação a ser extraída (ex: `0` para Quaternion, `39` para Acelerômetro). 

&#x20;\* `<imu\_id>` (int): O ID da IMU cujos dados você quer validar e extrair. 

&#x20;\* `<streaming\_slots>` (list): A lista retornada pela função `configure\_imu` indicando a ordem estrutural dos dados. 

&#x20;\* `\[usb]` (bool): Flag auxiliar para redirecionar a extração se for conexão direta USB (Padrão: `False`). 

\* \*\*Retorno:\*\* `value`: O dado extraído e formatado.





\---





\### \*\*`stop\_streaming(<serial\_port>, <imu\_ids>)`\*\*





Encerra o envio contínuo de dados e limpa os buffers de comunicação.





\* \*\*Parâmetros:\*\*`<serial\_port>`: A porta serial ativa. 

&#x20;\* `<imu\_ids>` (list): Lista de IDs das IMUs que terão o streaming interrompido.



