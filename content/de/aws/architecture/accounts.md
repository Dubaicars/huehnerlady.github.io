---
title: "Accountstruktur"
description: "Übersicht über und Motivation für die drei AWS Accounts der PKU."
---

Die Microservices, Datenbanken, Infrastruktur Services der PKU sind in
unterschiedlichen AWS Accounts deployed. Ein AWS Account ist das gröbste
Strukturierungselement, das AWS bietet. Accounts sind komplett isoliert
voneinander. Sie verfügen über eigene Netzwerke, ein eigenes
Berechtigungsmanagement, getrennte Abrechnungen usw. Eine Multi-Account
Strategie, wie wir sie bei uns nutzen, gilt seit einiger Zeit als [Best Practice
bei AWS](https://docs.aws.amazon.com/controltower/latest/userguide/aws-multi-account-landing-zone.html).
Das Konzept ist aber nicht AWS spezifisch. Bei Microsost Azure findet man
ganz ähnliche Konzepte, die z.B. im Rahmen einer [Landing Zone](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/enterprise-scale/architecture#landing-zone-in-enterprise-scale) aufgebaut werden.

Ultimativ ist das Ziel einer Multi-Account Strategie den Überblick zu wahren
und eine möglichst sichere Umgebung für unterschiedliche Workloads zur
Verfügung zu stellen (Mehr dazu [im Deep Dive](#multi-account-deep-dive)).

## Die PKU Accounts

![AWS Accounts der PKU](/aws/architecture/accounts.svg)

Die obige Abbildung zeigt die AWS Accounts der PKU. *PKU-Prod* und *PKU-Dev*
sind die beiden Stages Development und Production. Neben den Kreditsmart
Microservices werden hier auch Infrastruktur Services wie die CouchDB, PostgreSQL,
RabbitMQ usw. betrieben. Einmal in Dev und einmal in Prod.

Im *DevOps* Account befinden sich CDK Pilelines, die Workloads in mehrere Accounts
deployen können, weiteres CI/CD Tooling, wie z.B. GitHub Action Runners und
Infrastruktur Services wie Rundeck usw. 

Als Entscheidungskriterium hilft es darüber nachzudenken, ob eine Workload klar
Dev bzw. Prod zugeteilt werden kann. Im Falle von Microservcies, Datenbanken
und Queues ist dies relativ einfach mit "Ja" zu beantworten. Wenn dem so ist,
dann gehört sie in den entsprechenden Account.
Muss auf mehrere Accounts/Stages zugegriffen werden, wie bei Rundeck, dann
gehört die Workload in den DevOps Account.

Der DevOps Account kann auf die Netzwerke der beiden PKU Accounts zugreifen.
Zwischen den PKU Dev und Prod Accounts wird dies in naher Zukunft nicht mehr
möglich sein, um beispielsweise möglichen Schaden durch Fehlkonfiguration zu
vermeiden.

## Multi-Account Deep-Dive

Eine Multi-Account Strategie verfolgt verschiedene Ziele. Im Folgenden werden
zur Illustration nur einige davon aufgeführt.

Hinsichtlich Sicherheit sind mehrere Accounts sinnvoll, um den Blast Radius zu
reduzieren. Dringt ein Angreifer in einen AWS Account ein, so kann er dort im
schlimmsten Fall nicht nur alle Datenbanken löschen oder eine Cryptomining Farm
aufbauen, die schnell sehr teuer wird. Er kann auch alle Logs und Audit Trails
löschen und damit kann man als Firma nicht mehr beweisen, dass ein Angriff
stattgefunden hat. Mit mehreren Accounts kann der Schaden immerhin begrenzt
werden.

Dabei ist es aber möglich Accounts miteinander zu verbinden. Zum Beispiel ist das
Netzwerk bei uns so konfiguriert, dass alle Accounts aufeinander zugreifen
können. Auch andere Ressourcen wie Datenbanken, Queues usw. können über mehrere
Accounts geteilt werden. Dies erfordert aber eine Freigabe auf der einen Seite
und die Bestätigung der Freigabe auf der anderen.

Kosten sind ein weiterer Punkt. Üblicherweise teilen sich viele Anwendungen,
die über die verschiedensten Kostenstellen abgerechnet werden, einen Account.
Die Zuteilung zur Kostenstelle erfolgt dann meist über Tags an den Cloud
Ressourcen. Aber was wenn eine Ressources einmal nicht getaggt ist? Rechnet man
auf Basis der Accounts, so ist die Zuordnung der Kosten zu einer Kostenstelle
zuverlässiger möglich.

