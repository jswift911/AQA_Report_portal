# Установка Report Portal через Docker на Windows

Инструкция по установке Report Portal используя Docker на операционной системе Windows.

## Предварительные требования

- Docker Desktop для Windows
- Минимум 8 ГБ оперативной памяти

## Установка

1. Создайте новый проект на Gradle, либо склонируйте его
2. Добавьте настройки для Gradle которые содержат зависимости для Report Portal

```
plugins {
    id 'java'
}

group 'ru.netology'
version '1.0-SNAPSHOT'

sourceCompatibility = 11
compileJava.options.encoding = 'UTF-8'
compileTestJava.options.encoding = 'UTF-8'

repositories {
    mavenLocal()
    mavenCentral()
    jcenter()
    maven { url "http://dl.bintray.com/epam/reportportal" }

}

dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter:5.8.2'
    testImplementation 'com.codeborne:selenide:6.13.0'
    implementation 'com.github.javafaker:javafaker:1.0.2'
    implementation 'com.epam.reportportal:agent-java-junit5:5.1.6'
    implementation 'com.epam.reportportal:logger-java-logback:5.1.4'
    implementation 'ch.qos.logback:logback-classic:1.4.6'
    implementation 'com.epam.reportportal:logger-java-log4j:5.0.2'
    compileOnly 'log4j:log4j:1.2.17'
    implementation 'org.apache.logging.log4j:log4j-api:2.13.3'
    implementation 'org.apache.logging.log4j:log4j-core:2.13.3'
}

test {
    testLogging.showStandardStreams = true
    useJUnitPlatform()
    systemProperty 'selenide.headless', System.getProperty('selenide.headless')
    systemProperty 'junit.jupiter.extensions.autodetection.enabled', true
}

```

3. Откройте файл `docker-compose.yml` и скопируйте туда настройки с официального сайта https://reportportal.io/

4. В файле `docker-compose.yml` раскомментируйте настройки под вашу операционную систему (в нашем случае Windows):
```  
volumes:
# For windows host
  - postgres:/var/lib/postgresql/data
# For unix host
# - ./data/postgres:/var/lib/postgresql/data
```

```
volumes:
# For unix host
# - ./data/storage:/data 
# For windows host
 - minio:/data
```

```
# Docker volume for Windows host
volumes:
  postgres:
  minio:
```

5. Запустите следующую команду для запуска Report Portal:

```
docker-compose -p reportportal up -d --force-recreate   
```

6. Откройте ваш браузер и перейдите по адресу http://localhost:8080/ui/

![Annotation 2023-06-13 140157](https://github.com/jswift911/AQA_Report_portal/assets/46243492/bbfc649d-e7da-497e-8353-1b24877215f5)


7. Войдите в систему с логином `superadmin` и паролем `erebus`

8. Зайдите в настройки профиля и скопируйте Ваши персональные настройки к себе в проект, в файл `reportportal.properties`

![Annotation 2023-06-13 140136](https://github.com/jswift911/AQA_Report_portal/assets/46243492/100a41bc-ffac-4d72-9df3-de2985fbf3c2)


9. Запустите любой тест из проекта для проверки того, что проект связался с Report Portal успешно 
 - в моем случае это <b>shouldRegisterNewDate()</b>

![Annotation 2023-06-13 140229](https://github.com/jswift911/AQA_Report_portal/assets/46243492/37703dc2-40a8-43ed-b0cb-869955f78240)

10. Убедитесь, что проект действительно связался проверив запущеные тесты в разделе `Запуски`

![Annotation 2023-06-13 140113](https://github.com/jswift911/AQA_Report_portal/assets/46243492/57bf9b1d-5c96-4532-9868-0c61b37f9b5c)

Готово!
