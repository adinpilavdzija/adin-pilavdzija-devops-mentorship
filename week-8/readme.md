Description: TASK-7: Autoscaling Group and Load Balancer 

- Stavke koje su odradjene u ovom tasku:
  - [x] Kreiranje AMI image od instance `ec2-vedad-nuhic-web-server`, AMI image nazovite `ami-vedad-nuhic-web-server`.
  - [x] Kreiranje Application Load Balancer naziva `alb-web-servers` koji ce biti povezan sa Target Group `tg-web-servers`.
  - [x] Kreiranje ASG sa MIN 2 i MAX 4 instance. Tip instance koji sam koristio unutar ASG je `t2.micro` ili `t3.micro` gdje sam koristio `alb-web-servers` Load Balancer. ASG bi trebala da skalira prema gore (scale-up) kad CPU predje `18%` i skalira prema dole (scale down) kad god CPU Utilisation padne ispod `18%`.
  - [x] Security grupe dozvoljavaju najmanje potrebne otvorene portove.
  - [x] Simulirana visoka dostupnost na nacin da su terminirane instance.
  - [x] Simuliran CPU load prateci korake iz tutorijala [https://www.wellarchitectedlabs.com/performance-efficiency/100_labs/100_monitoring_linux_ec2_cloudwatch/5_generating_load/]

AMI image
![1](https://user-images.githubusercontent.com/65655945/233834143-98c231f8-a130-412c-9504-e592dd397f62.png)

ALB i Target group
![2](https://user-images.githubusercontent.com/65655945/233834193-16d09fd5-e8dd-4f29-a413-69b624291659.png)

ASG
![3](https://user-images.githubusercontent.com/65655945/233834208-284b56f1-7a1b-4e1b-ab20-1911c6006614.png)

Simulirana velika dostupnost
![4](https://user-images.githubusercontent.com/65655945/233834221-cf41ed5d-e20a-4771-bd37-9084db933b41.png)

Simuliranje CPU Load (stress utility)
![5](https://user-images.githubusercontent.com/65655945/233834244-4c1821a3-05b0-4ddc-b199-232df934a843.png)

Dijagram infrastrukture
![drawio](https://user-images.githubusercontent.com/65655945/233834284-e27feb48-ddf5-434c-8e54-f82cc5c139ce.jpg)

DNS record Load Balancera
![6](https://user-images.githubusercontent.com/65655945/233834271-746522bc-d647-4c94-84e7-4e2af3ee0b61.png)
alb-web-servers-1618028797.eu-central-1.elb.amazonaws.com
