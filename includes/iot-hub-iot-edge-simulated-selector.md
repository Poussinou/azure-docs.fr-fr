> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md)
> 
> 

Cette procédure pas à pas de [l’exemple de chargement d’un appareil simulé sur le cloud] montre comment utiliser [Azure IoT Edge][lnk-sdk] pour envoyer les données de télémétrie appareil-vers-cloud à IoT Hub à partir d’appareils simulés.

Cette procédure pas à pas inclut les étapes suivantes :

1. **Architecture**: informations architecturales importantes concernant l'exemple de chargement d’un appareil simulé sur le cloud.
2. **Créer et exécuter**: les étapes requises pour créer et exécuter l'exemple.

## <a name="architecture"></a>Architecture
L’exemple de chargement d’un appareil simulé sur le cloud montre comment utiliser IoT Edge pour créer une passerelle qui envoie les données de télémétrie d’appareils simulés vers un hub IoT. Les appareils simulés ne peuvent pas se connecter directement à IoT Hub car :

* Les appareils n'utilisent pas un protocole de communication compris par IoT Hub.
* Les appareils ne sont pas suffisamment intelligents pour mémoriser l'identité attribuée par IoT Hub.

La passerelle IoT Edge résout ces problèmes pour les appareils simulés comme suit :

* La passerelle comprend le protocole utilisé par les appareils simulés, reçoit les données de télémétrie appareil-vers-cloud à partir des appareils, puis transfère ces messages vers IoT Hub à l'aide d'un protocole compris par IoT Hub.
* La passerelle stocke les identités IoT Hub pour le compte des appareils simulés et agit comme un proxy lorsque les appareils simulés envoient des messages à IoT Hub.

Le diagramme suivant montre les principaux composants de l’exemple, notamment les modules IoT Edge :

![][1]

> [!NOTE]
> Les modules ne s’échangent pas directement les messages. Les modules publient des messages vers un répartiteur interne qui remet les messages aux autres modules à l’aide d’un mécanisme d’abonnement comme indiqué dans le diagramme ci-dessous. Pour plus d’informations, voir [Get started with Azure IoT Edge (Prise en main d’Azure IoT Edge)][lnk-gw-getstarted].
> 
> 

### <a name="protocol-ingestion-module"></a>Module d'ingestion de protocole
Ce module est le point de départ pour transférer des données depuis des périphériques vers le cloud, via la passerelle. Dans l'exemple, le module effectue quatre tâches :

1. Il crée les données de la température simulée. Remarque : si vous utilisez des périphériques réels, le module lit les données à partir de ces périphériques physiques.
2. Il place les données de la température simulée dans le contenu d'un message.
3. Il ajoute une propriété avec une adresse MAC factice au message contenant les données de la température simulée.
4. Il rend le message disponible au module suivant dans la chaîne.

> [!NOTE]
> Le module appelé **Protocol X ingestion** dans le diagramme ci-dessus est appelé **Simulated device** dans le code source.
> 
> 

### <a name="mac-lt-gt-iot-hub-id-module"></a>MAC &lt;-&gt; IoT Hub ID module
Ce module recherche les messages qui incluent une propriété contenant l'adresse MAC, ajoutée par le module d'ingestion de protocole, de l’application de périphérique simulé. Si le module détecte une telle propriété, il ajoute une autre propriété avec une clé d’appareil IoT Hub au message, puis rend le message disponible au module suivant dans la chaîne. Voici comment l'exemple associe les identités des appareils IoT Hub aux appareils simulés. Le développeur configure manuellement le mappage entre les adresses MAC et les identités IoT Hub dans le cadre de la configuration du module. 

> [!NOTE]
> Cet exemple utilise une adresse MAC comme identificateur unique de l'appareil et la met en corrélation avec une identité d'appareil IoT Hub. Toutefois, vous pouvez écrire votre propre module qui utilise un autre identificateur unique. Par exemple, vous pouvez avoir des appareils avec un numéro de série unique ou des données de télémétrie qui utilisent un nom d’appareil unique incorporé permettant de déterminer l’identité de l’appareil IoT Hub.
> 
> 

### <a name="iot-hub-communication-module"></a>Module de communication IoT Hub
Ce module reçoit les messages dont l’identité d’appareil IoT Hub est attribuée par le module précédent puis envoie le contenu du message vers IoT Hub à l'aide du protocole HTTP. HTTP est un des trois protocoles compris par IoT Hub.

Au lieu d'ouvrir une connexion vers IoT Hub pour chaque application de périphérique simulé, ce module ouvre une connexion HTTP à partir de la passerelle vers IoT Hub et multiplexe des connexions à partir de tous les appareils simulés sur cette connexion. Cela permet à une passerelle unique de connecter de nombreux autres appareils, simulés ou non, comme si une connexion unique était ouverte pour chaque appareil.

![][2]

<!-- Images -->
[1]: media/iot-hub-iot-edge-simulated-selector/image1.png
[2]: media/iot-hub-iot-edge-simulated-selector/image2.png

<!-- Links -->
[l’exemple de chargement d’un appareil simulé sur le cloud]: https://github.com/Azure/iot-edge/blob/master/samples/simulated_device_cloud_upload/README.md
[lnk-sdk]: https://github.com/Azure/iot-edge
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md