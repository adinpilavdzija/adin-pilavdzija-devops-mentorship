# Komande koristene u izradi taska 8

`$ aws route53 change-resource-record-sets --hosted-zone-id Z3LHP8UIUC8CDK --change-batch '{"Changes":[{"Action":"UPSERT","ResourceRecordSet":{"Name":"adin-pilavdzija.awsbosnia.com.","Type":"A","TTL":60,"ResourceRecords":[{"Value":"52.59.103.129"}]}}]}'` *#kreiran DNS record za Hosted Zone awsbosnia.com*

`$ aws route53 list-resource-record-sets --hosted-zone-id Z3LHP8UIUC8CDK | jq '.ResourceRecordSets[] | select(.Name == "adin-pilavdzija.awsbosnia.com.") | {Name, Value}'` *#ispisivanje Name i Value DNS recorda pomocu jq*

`$ sudo dnf install python3 augeas-libs`  
`$ sudo python3 -m venv /opt/certbot/`  
`$ sudo /opt/certbot/bin/pip install --upgrade pip`  
`$ sudo /opt/certbot/bin/pip install certbot certbot-nginx`  
`$ sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot`  
`$ sudo ls -la /usr/bin/certbot`  
`$ sudo certbot certonly --nginx`  
*#kreiranje Let's Encrypt certifikata pomocu alata certbot*

`$ SLEEPTIME=$(awk 'BEGIN{srand(); print int(rand()*(3600+1))}'); echo "0 0,12 * * * root sleep $SLEEPTIME && certbot renew -q" | sudo tee -a /etc/crontab > /dev/null` *#autorenewal preko certbota*

`$ openssl s_client -showcerts -connect adin-pilavdzija.awsbosnia.com:443` #prikazivanje ssl certifikata

`$ aws route53 change-resource-record-sets --hosted-zone-id Z3LHP8UIUC8CDK --change-batch '{"Changes":[{"Action":"CREATE","ResourceRecordSet":{"Name":"_0745668a2195a19ca010392e8fb6cd97.adin-pilavdzija.awsbosnia.com.","Type":"CNAME","TTL":60,"ResourceRecords":[{"Value":"_f3551e198d743dc791c1934d64fa3289.fcgjwsnkyp.acm-validations.aws."}]}}]}'` *#dodavanje kredencijala novog Amazon certifikata unutar AWS CLI*

`$ aws route53 change-resource-record-sets --hosted-zone-id Z3LHP8UIUC8CDK --change-batch '{"Changes":[{"Action":"DELETE","ResourceRecordSet":{"Name":"adin-pilavdzija.awsbosnia.com.","Type":"A","TTL":60,"ResourceRecords":[{"Value":"52.59.103.129"}]}}]}'` *#brisanje postojeceg recorda zbog errora*

`$ aws route53 change-resource-record-sets --hosted-zone-id Z3LHP8UIUC8CDK --change-batch file:///home/ec2-user/json-file.json` *#kreiranje novog recorda sa domenom i DNS recordom ALB, json-file.json uploadovan u week-9*

`$ echo | openssl s_client -showcerts -servername adin-pilavdzija.awsbosnia.com -connect adin-pilavdzija.awsbosnia.com:443 2>/dev/null | openssl x509 -inform pem -noout -text` *#prikaz SSL certifikata*