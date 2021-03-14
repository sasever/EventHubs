# Event Hubs

In this scenario we will be creating below objects:

1. [A resource group that will contain all our hack objects](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal)
2. [A Virtual Network that will have the Client VM subnet](
     https://docs.microsoft.com/en-us/azure/virtual-network/quick-create-portal)
3. [A Data Science Virtual Machine for Windows 2019 that will be hosting our client applications](https://docs.microsoft.com/en-us/azure/machine-learning/data-science-virtual-machine/provision-vm)
4. [An Event Hubs Name Space & an EventHub under it](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create)
5. [A private link that is connecting the clientVMs subnet to Event Hubs Namespace](https://docs.microsoft.com/en-us/azure/event-hubs/private-link-service)

Once we create  above objects we can start working on our client VM. We specifically used Data Science VMs since it comes preinstalled with Jupyter notebooks and python which our client appications will be executed with.

For the client VM we have 4  python notebooks:

Producers:

   1. [Producer with Event Hub Native Python SDK](Notebooks/ProducerEhubNative.ipynb)
   2. [Producer with Confluent Kafka Python package](Notebooks/KafkaProducerEhub.ipynb)

Consumers:

   1. [Consumer with Event Hub Native Python SDK](Notebooks/ConsumerEhubNative.ipynb)
   2. [Consumer with Confluent Kafka Python package](Notebooks/KafkaConsumerEhub.ipynb)
 
## IMPORTANT!!:

There are some prerequisites to be able to run our Kafka producer and consumer. Using Event Hubs as Kafka requires SSL certificates and SSL software to be present in the client environment. Also [confluent-kafka python package](https://docs.confluent.io/clients-confluent-kafka-python/current/overview.html) uses  [librdkafka](https://github.com/edenhill/librdkafka) . **librdkafka ** is a C library implementation of the Apache Kafka protocol, providing Producer, Consumer and Admin clients with ssl support, which we require for confluent-kafka package work with event hubs.

1. We need to have valid SSL certificate.
    * You can use [Mozilla CA Certificates](https://curl.se/docs/caextract.html) for this purpose.
    * You  need to store the certificate under an easy path with no spaces like **C:\\Certificates**
    * The certificate you download is named **cacert.pem** you can use it as it is or also change the extension to **.crt** both will work unless you provide the correct path.

1. To be able to use SSL certificates we need to install SSL software.
    * You can use [OpenSSL Windows Client](https://slproweb.com/products/Win32OpenSSL.html) for this purpose.

1. We need to install  [librdkafka](https://github.com/edenhill/librdkafka) for confluent-kafka package to be able to work. **librdkafka ** is a C library implementation of the Apache Kafka protocol.
    * To install  librdkafka we can simply use [VCPKG package manager](https://github.com/Microsoft/vcpkg#getting-started) for an IDE independent easy installation  on windows.
        *  to install  VCPKG package manager:

        ```console
         git clone https://github.com/microsoft/vcpkg
         .\vcpkg\bootstrap-vcpkg.bat
        ```

        * then to install librdkafka
        ```console
        .\vcpkg\vcpkg install librdkafka
        ```

Now you are Ready to Run the Kafka Consumer and Producer.



*Resources used:*
* *https://docs.microsoft.com/en-us/azure/event-hubs/apache-kafka-developer-guide*
* *https://pypi.org/project/azure-eventhub/*
