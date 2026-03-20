# Documentação


### **`initialize_dongle(<imu_ids>)`**


Inicializa a conexão serial com o Dongle (receptor sem fio) conectado ao computador.


* **Parâmetros:**`<imu_ids>` (list): Lista com os IDs (números inteiros) das IMUs que se comunicarão com o Dongle. 
* **Retorno:**`serial_port`: Objeto de conexão serial configurado para comunicação.


---


### **`initialize_sensor(<imu_ids>)`**


Inicializa a conexão serial diretamente com um sensor IMU conectado via cabo USB ao computador.


* **Parâmetros:**`<imu_ids>` (list): Lista com o ID do sensor conectado. 
* **Retorno:**`serial_port`: Objeto de conexão serial configurado.


---


### **`configure_imu(<serial_port>, <imu_ids>, [opcionais...])`**


Configura os parâmetros internos do sensor (filtros, calibração, tara) e define quais pacotes de dados serão enviados durante o streaming.


* **Parâmetros:**`<serial_port>`: A porta serial ativa (Dongle ou Sensor). 
 * `<imu_ids>` (list): Lista de IDs lógicos das IMUs sendo configuradas. 
 * `[disableCompass]` (bool): Desabilita o magnetômetro (Padrão: `True`). 
 * `[disableGyro]` / `[disableAccelerometer]` (bool): Desabilitam giroscópio e acelerômetro, respectivamente (Padrão: `False`). 
 * `[gyroAutoCalib]` (bool): Calibra o giroscópio automaticamente. O sensor deve estar estático (Padrão: `True`). 
 * `[tareSensor]` (bool): Tara a orientação do sensor usando o quaternion atual. Requer o sensor plano e apontado para você (Padrão: `True`). 
 * `[tareWithQuaternion]` (dict): Dicionário (ex: `{'imu8': [w, x, y, z]}`) para tarar a orientação com um quaternion específico (Padrão: `None`). 
 * `[filterMode]` (int): Define o filtro de orientação (0: Sem filtro, 1: Kalman [Padrão], 2: Q-COMP, 3: Q-GRAD). 
 * `[baudrate]` (int): Taxa de transmissão de dados (Padrão: `115200` bps). 
 * **Flags de Streaming (bool):** `[show_quaternion]` (Padrão: `True`), `[show_euler_angle]` (Padrão: `True`), `[show_accel]` (Padrão: `False`), `[show_gyro]` (Padrão: `False`), `[show_compass]` (Padrão: `False`), `[show_rotation_matrix]` (Padrão: `False`), `[show_button]` (Padrão: `False`). Definem quais dados a IMU enviará. 
* **Retorno:** `streaming_commands` (list): Lista com os códigos dos dados solicitados.


---


### **`start_streaming(<serial_port>, <imu_ids>, <frequency>, [timestamp], [duration], [delay])`**


Inicia o envio contínuo de dados pelas IMUs configuradas de acordo com os parâmetros estabelecidos.


* **Parâmetros:** `<serial_port>`: A porta serial ativa. 
 * `<imu_ids>` (list): Lista de IDs das IMUs que iniciarão o streaming. 
 * `<frequency>` (int): Frequência de envio de dados em Hz. Valor `0` envia o mais rápido possível. 
 * `[timestamp]` (bool): Inclui carimbo de tempo nos dados (Padrão: `False`). 
 * `[duration]` (int): Duração do streaming. Por padrão, ocorre por tempo indefinido. 
 * `[delay]` (int): Atraso em segundos antes de iniciar o fluxo de dados (Padrão: `0`).


---


### **`read_data(<serial_port>)`**


Lê as informações brutas enviadas pela IMU que estão aguardando no buffer da porta serial.


* **Parâmetros:** `<serial_port>`: A porta serial de onde os dados serão lidos. 
* **Retorno:** data` (bytes): Os dados brutos capturados do sensor. Retorna `None` se não houver dados.


---


### **`extract_data(<data>, <type_of_data>, <imu_id>, <streaming_slots>, [usb])`**


Interpreta o pacote de bytes bruto recebido e extrai a informação matemática/física desejada.


* **Parâmetros:** `<data>` (bytes): Os dados brutos retornados pela função `read_data`. 
 * `<type_of_data>` (int): O código do tipo de informação a ser extraída (ex: `0` para Quaternion, `39` para Acelerômetro). 
 * `<imu_id>` (int): O ID da IMU cujos dados você quer validar e extrair. 
 * `<streaming_slots>` (list): A lista retornada pela função `configure_imu` indicando a ordem estrutural dos dados. 
 * `[usb]` (bool): Flag auxiliar para redirecionar a extração se for conexão direta USB (Padrão: `False`). 
* **Retorno:** `value`: O dado extraído e formatado.


---


### **`stop_streaming(<serial_port>, <imu_ids>)`**


Encerra o envio contínuo de dados e limpa os buffers de comunicação.


* **Parâmetros:**`<serial_port>`: A porta serial ativa. 
 * `<imu_ids>` (list): Lista de IDs das IMUs que terão o streaming interrompido.
