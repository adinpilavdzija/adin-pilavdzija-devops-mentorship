<h1 align="center">DevOps Mentorship Program</h1>

![adinpilavdzija-awsbosnia-devops-mentorship](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/2ab6ec52-6a0a-412e-a35b-be9a2c3bf5a6)

Ovaj repozitorij sadrzi rjesenja taskova i biljeske vezane za [DevOps Mentorship Program by AWS Bosnia](https://github.com/awsbosnia/devops-aws-mentorship-program). Ovaj repozitorij minimalno odstupa od zahtjeva taskova (uglavnom u nazivima pojedinih fajlova i direktorija) zbog bolje organizacije i preglednosti. 

Svi resursi vezani za [FREE DevOps Mentorship Program](https://github.com/awsbosnia/devops-aws-mentorship-program) dostupni su na:
- https://github.com/awsbosnia/devops-aws-mentorship-program
- https://devops.awsbosnia.com/
- [AWS Bosnia slack workspace](https://join.slack.com/t/awsbih/shared_invite/zt-ad8kr3c7-mcFYB~s9SRdEjulMo141dw)

> [!NOTE]
DevOps Mentorship Program je mentorski program pokrenut od strane grupe entuzijasta, AWS zajednice u Bosni i Hercegovini te uz pomoc kolega iz AWS zajednice Crne Gore sa ciljem sirenja znanja i pomoci svima onima koji zele da svoju IT karijeru grade i razvijaju u navedenom podrucju.<br><br>
Po definiciji, DevOps kao radno mjesto odnosno pozicija ne postoji, ono sto postoji je DevOps kultura i pokret koji se oslanja na upotrebu razlicitih alata i vjestina kako bi se pomoglo brzoj isporuci softvera. Upravo iz tog razloga, mentorship program podjednako obuhvata razvoj tehnickih i "soft" skills.<br><br>
Pored toga sto ima za cilj da kroz predavanja odrzana za DevOps Mentorship Program pomogne svima onima koji zele da naprave svoje prve korake u AWS i DevOps svijetu, cilj ovog repozitorija je da ponudi jedinstveni DevOps Learning Path sa biljeskama i uputama pisanim na nasem jeziku.

`DevOps Mentorship Program` sastoji se od:
- sedmicnih predavanja (Na pocetku dva sedmicna predavanja (utorak i cetvrtak) o istoj temi za dvije odvojene grupe polaznika uz objavljivanje video materijala i opciju gledanja drugog predavanja zbog mogucih sitnih razlika. U glavnom dijelu, dva razlicita sedmicna predavanja za sve polaznike. Na samom kraju, jedno sedmično predavanje za sve polaznike.)
- izrade taskova (pregledanih od strane mentora)
- Office Hours sesija (svake druge subote rekapitulacija obradjenih tema, Q&A, dodatne upute)
- slack komunikacije sa kolegama i mentorima (AWS Bosnia slack workspace)
- koristenja obaveznih 'class notes' materijala
- koristenja preporucenih dodatnih materijala za ucenje (knjige, blogovi, video resursi) 

Predavanja i taskovi obuhvataju, izmedju ostalih, sljedece tehnologije:  
<img src="https://skillicons.dev/icons?i=aws,linux,bash,vim,vscode,git,github,md,docker,jenkins,nginx,java,nodejs,dynamodb,mysql,postgres,ansible"/>

## Teme predavanja

<table align="center">
<tr>
<td>

Week 1: Git i Github  
Week 2: Linux  
Week 3: Shell & Bash scripting  
Week 4: Racunarske mreze, OSI Model, Web aplikacije, TCP, HTTP, SSL/TLS, DNS  
Week 5: Server, tipovi servera, nginx, Apache  
Week 6: Uvod u Cloud, AWS servisi  
Week 7: Uvod u AWS, EC2, IAM, AWS CLI  
Week 8: Amazon EBS, Amazon ASG, AMI Image, Security Groups  
Week 9: Amazon S3, Amazon RDS  
Week 10: Amazon RDS, Deploying Java App on AWS  
Week 11: Amazon VPC, Amazon CloudFront  
Week 12: AWS Lambda, EventBridge, SNS, SQS, API Gateway  
Week 13: DevOps Culture and Practices (Viktor Farcic, Urban Jurca), AWS Elastic Beanstalk  
Week 14: Server Management alati, Packer, Ansible, Base images  
Week 15: Infrastructure as Code (IaC), CloudFormation, Terraform  
Week 16: AWS CDK, AI on AWS  
Week 17: Continuous Integrations and Continuous Delivery/Deployment (CI/CD)  
Week 18: Jenkins, Disaster Recovery on AWS  
Week 19: Docker  
Week 20-23: Building DevOps Blue/Green pipeline with Amazon ECS  
Week 24: Relational Databases on AWS  
Week 25: Software Engineering Practices  
Week 26: NoSQL Databases on AWS  
Week 27: Billing and Cost Management  
Week 28: Elastic Kubernetes Service (EKS)  
</td>
</tr>
</table>

## Taskovi

🔴
- []() - Kreiranje GitHub repozitorija za potrebe mentorship programa. Dodavanje `.gitignore` i `README.md` fajlova. Kreiranje brancheva `development` i `main` uz zabranu direktnih commitova na `main`.
- []() - Izrada 10 taskova Bandit levela na `Wargames labs` uz koritenje Linux komandi.
- []() - Čitanje poglavlja 13, 14, 15 i 16 knjige `Linux Command Line and Shell Scripting Bible, 3rd Edition` uz `ssh` na remote server, kreiranje vlastitog direktorija i testiranje bash skripti iz navedenih poglavlja knjige.
- []() - Čitanje materijala vezanih za `računarske mreže` sa naglaskom na OSI model, protokole, IP adresiranje, klijent-server arhitekturu, DNS, subnetiranje i sl.
- []() - Kreiranje `AWS Account`-a, postavljanje `MFA`, `alias`, kreiranje `IAM Usera` i `IAM Group`-e...
- []() - Kreiranje `CloudWatch billing alert`-a, kreiranje `EC2 instance` sa zadanim postavkama, deployment jednostavne `nodejs` aplikacije na EC2 instancu
- []() - Kreiranje `AMI Image`-a, `Application Load Balancer`-a, `Auto Scaling Group`-e, `dijagram infrastrukture` na draw.io, simuliranje visoke dostupnosti aplikacije i CPU load-a, upravljanje `Security Group`-om
- []() - Kreiranje `EC2` instance od `AMI Image-a`, kreiranje `DNS recorda`, kreiranje `Let's Encrypt SSL certifikata` za domenu i omogucavanje njegovog `autorenewal-a`, importovanje Lets Encrypt SSL certifikat unutar `AWS Certificate Managera`, kreiranje `Load Balancera`, kreiranje novog SSL certifikata unutar `ACM`-a
- []() - Podesavanje statickog website-a pomocu AWS servisa `S3` i `Cloudfront`, koristenje SSL certifikata od `AWS Certificate Managera`, kreiranje record-a unutar `Route 53` kroz `AWS CLI`
- []() - Ucenje, pravljenje zabiljeski i vjezbanje DEMO-a iz poglavlja `Serverless and Application Services` iz kursa Adriana Cantrill-a `AWS Certified Solutions Architect - Associate (SAA-C03)`. Neke od lekcija su: `AWS Lambda`, `CloudWatchEvents and Event Bridge`, `Serverless Architecture`, `SNS`, `SQS` itd.
- []() - Izrada `AWS Tools GitFlow Workshop` unutar vlastitog AWS Account-a
- []() - Koristenje `Packer ⇨ CloudFormation / Terraform ⇨ Ansible` alata i kreiranje paralelnih infrastruktura (jedne koristeci `CloudFormation` a druge koristeci `Terraform`) sa dvije EC2 instance kao Web (nginx) i Database (mysql) serverima
- []() - Izvrsavanje Workshop lab-ova vezanih za `AWS CodeCommit`, `AWS CodeArtifact`, `AWS CodeBuild`, `AWS CodeDeploy` i `AWS CodePipeline`

![dev_ops_awsbosnia](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/94a7dc05-a7b6-42b7-b3e3-45708ab4ac9c)