# Process logic

The Process logic describe the autoconfiguration process of the components services.

In fact, each components, such as User component define a set of metadata to create the service dependencies.

Services dependencies are :

 * dispatcher
 * provider
 * formatter
 * serializer
 * name converter
 * controller

## Get multiples services

The GET\_MUTLIPLES process will inject :

 * name converter into serializer
 * serializer into formatter
 * formater as dispatcher listener
 * provider as dispatcher listener
 * dispatcher into controller

## Auto-configuration system

The auto-configuration define some metadata to handle the service creation.

 * ControllerMetadata
 * DispatcherMetadata
 * EventMetadata
 * FormatterMetadata
 * ProcessMetadata
 * ProviderMetadata
 * SerializerMetadata

Each of this metadata offer it's own builder. These builders allow to create a Metadata instance, but ProcessMetadata builder that create a set of ProcessMetadata.

To format the complete configuration, a FormatHandler instance is used, in order to process the validations. This FormatHandler will return an array, used by ProcessBuilder to create the metadatas.

To load the format, a PHPLoader or YamlLoader instance can be used.

```php
$file = 'file.yaml';

$phpLoader = new PHPLoader();
$yamlLoader = new YamlLoader($phpLoader, new Yaml(), Yaml::PARSE_CONSTANT);

if ($yamlLoader->support($file)) {
    $yamlLoader->addMetadata($file);

    $data = $yamlLoader->load();

    $priorityValidator = new PriorityValidator();
    $strategy = new AffirmativeStrategy();
    $callable = new CallableListenerValidator($priorityValidator);
    $subscriber = new SubscriberListenerValidator();
    $serviceManager = new ValidationManager($strategy);
    $serviceManager->addValidator($callable);
    $serviceManager->addValidator($subscriber);

    $manager = new ValidationManager(new ReversionStrategy($strategy));
    $manager->addValidator($callable);
    $manager->addValidator(new FunctionListenerValidator($priorityValidator));
    $manager->addValidator(new ServiceListenerValidator($container, $serviceManager));
    $manager->addValidator($subscriber);

    $formatHandler = new FormatHandler($manager);

    $metadatas = $formatHandler->handleData($data);

    $builderFactory = new BuilderFactory();
    $builder = $builderFactory->getBuilder();

    $processes = $builder->buildFromData($metadatas);
}
```
