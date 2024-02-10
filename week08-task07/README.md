# 07 Autoscaling Group and Load Balancer 

## Task

**IAM User 1** ce svoje resurse da kreira unutar `eu-central-1` regiona.  

Svaki od AWS resursa koje kreirate pored taga `Name` mora sadrzavati i tagove `CreatedBy: Ime Prezime` i `Email:vas@email.com`  
**NOTE: Ako nije explicitno navedeno AWS Account Owners / IAM User 1 / IAM User 2 / IAM User 3 zadatak se odnosi na sviju.**

*   Kreirajte AMI image od instance `ec2-ime-prezime-web-server`, AMI image nazovite `ami-ime-prezime-web-server`
*   Kreirajte Application Load Balancer pod nazivom `alb-web-servers` koji ce da bude povezan sa Target Group `tg-web-servers`
*   Kreirajte Auto Scaling group sa MIN 2 i MAX 4 instance. Tip instance koji cete koristiti unutar ASG je `t2.micro` ili `t3.micro` gdje cete koristiti `alb-web-servers` Load Balancer. AutoScaling group bi trebala da skalira prema gore (scale-up) kad god CPU predje 18% i da skalira prema dole (scale-down) kad god CPU Utilisation padne ispod 18%
*   Voditite racuna da security grups koje budete koristili nakon sto zavrsite sa zadatakom dozvoljavaju namanje potrebne otvorene portove.
*   Kreirajte free account na draw.io ili lucidchart.com stranicama i napravite dijagram infrastrukture iz ovog onako kako je vi vidite/razumijete.
*   Pokusajte simulirati visoku dostupnost vase aplikacije na nacin da terminirate instance.
*   Pokusajte simulirati CPU load prateci sljedeci [tutorijal](https://www.wellarchitectedlabs.com/performance-efficiency/100_labs/100_monitoring_linux_ec2_cloudwatch/5_generating_load/)

Kada zavrsite zadatak napravite Pull Request u kojem cete minimalno postaviti DNS record vaseg Load Balancera i dijagram infrastrukture koji ste kreirali i postavite link na PR kao komentar na ovaj task.

Kada dobijete approval na pull request neophodo je da terminirate / ocistite resurse koje ste keirali.

## Task solution

- [x] Kreiran AMI image od instance `ec2-adin-pilavdzija-web-server`, AMI image nazvan `ami-adin-pilavdzija-web-server`.

![1](https://user-images.githubusercontent.com/65655945/233834143-98c231f8-a130-412c-9504-e592dd397f62.png)

- [x] Kreiran Application Load Balancer (ALB) naziva `alb-web-servers` koji je povezan sa Target Group `tg-web-servers`.

![2](https://user-images.githubusercontent.com/65655945/233834193-16d09fd5-e8dd-4f29-a413-69b624291659.png)

- [x] Kreirana Auto Scaling Group (ASG) sa MIN 2 i MAX 4 instance. Tip instance koristen unutar ASG je `t2.micro` ili `t3.micro` sa `alb-web-servers` Load Balancer-om. ASG  skalira prema gore (scale-up) kad CPU predje `18%` i skalira prema dole (scale down) kad god CPU Utilization padne ispod `18%`.

![3](https://user-images.githubusercontent.com/65655945/233834208-284b56f1-7a1b-4e1b-ab20-1911c6006614.png)

- [x] Security grupe dozvoljavaju najmanje potrebne otvorene portove.
- [x] Kreiran dijagram infrastrukture
- [x] Simulirana visoka dostupnost na nacin da su terminirane instance.

![4](https://user-images.githubusercontent.com/65655945/233834221-cf41ed5d-e20a-4771-bd37-9084db933b41.png)

- [x] Simuliran CPU load prateci korake iz [tutorijala](https://www.wellarchitectedlabs.com/performance-efficiency/100_labs/100_monitoring_linux_ec2_cloudwatch/5_generating_load/)

![5](https://user-images.githubusercontent.com/65655945/233834244-4c1821a3-05b0-4ddc-b199-232df934a843.png)

## DNS record Load Balancera i dijagram infrastrukture

### DNS record Load Balancera
![6](https://user-images.githubusercontent.com/65655945/233834271-746522bc-d647-4c94-84e7-4e2af3ee0b61.png)
[alb-web-servers-1618028797.eu-central-1.elb.amazonaws.com](alb-web-servers-1618028797.eu-central-1.elb.amazonaws.com)

### Dijagram infrastrukture
![drawio](https://user-images.githubusercontent.com/65655945/233834284-e27feb48-ddf5-434c-8e54-f82cc5c139ce.jpg)
