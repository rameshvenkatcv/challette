# challette
REST API-Proj-v.01
1.REST API Java servlet service source code location , unzip Sample.zip

..\Sample\src\main\java\com\sample\code\SampleServlet.java

2.Location to Rest API ENDPOINT webapps ,  ..\Sample\target\Sample.war
3.To Containerize the REST API servlet service using DockerFile , create a Dockerfile and add below ,
FROM tomcat:8.5.12-jre8-alpine
VOLUME /tmp
COPY ./Webapp/Sample.war /usr/local/tomcat/webapps/Sample.war
EXPOSE 8080
CMD ["catalina.sh", "run"]
4.To build Dockerfile , use below commands,
docker build -t restapi .
docker run -dit -p 8080:8080 restapi:latest
5.To pull restapi docker image from repository,ignore 3 and 4 steps, use below to pull from public repo ,
docker pull rameshvenkat/testing:restapiv0.1
6.To deploy application in minikube or kubernetes , use below yaml content,

  apiVersion: apps/v1
kind: Deployment
metadata:
  name: script
spec:
  replicas: 1
  selector:
    matchLabels:
      app: script
  template:
    metadata:
      labels:
        app: script
    spec:
      subdomain: rest-api
      imagePullSecrets:
      - name: dockercred
      containers:
      - name: restapi
        resources:
        image: <dockerimagerepo>/restapiV0.1
        imagePullPolicy: Always
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: script
spec:
  type: LoadBalancer
  ports:
  - port: 8080
    protocol: TCP
    targetPort: 8080
  selector:
    app: script


To use Nodeport , use below, if using load balancer ignore below,

apiVersion: v1
kind: Service
metadata: 
name: script
spec:
selector: 
app: my-app
type: NodePort
ports: 
- name: http
port: 80
targetPort: 80
nodePort: 30036
protocol: TCP

7.To deploy application in kubernetes use below , 
 kubectl apply -f scripts.yaml
8.To check kubectl service  , use below, 
          kubectl get svc
          Kubectl get pods
   ip-111-33-33-33:~/restAPI$ kubectl get svc
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP                                                               PORT(S)          AGE
kubernetes   ClusterIP      10.100.0.1      <none>                                                                    443/TCP          116m
script       LoadBalancer   10.100.11.12   a2d9e4-201323447.uk-east-2.elb.amazonaws.com   8080:31043/TCP   94m
       

9.Connect Postman to check the restapi service ,
Request URL : http://<IP>:8080/Sample/SampleServlet?email=Delivered-To: santa@northpole.com
Received: by 10.159.41.68 with SMTP id t62csp570647uat;
Thu, 16 Mar 2017 04:44:28 -0700 (PDT)
X-Received: by 10.99.140.69 with SMTP id q5mr9342725pgn.179.1489664668601;
Thu, 16 Mar 2017 04:44:28 -0700 (PDT)
Return-Path: <llbean.511020444@envfrm.r1234s5.com>
Received: from omp.e1.llbean.com (omp.e1.llbean.com. [199.7.202.38])
by mx.google.com with ESMTPS id d2si234254576pli.110.2017.03.16.04.44.28
for <santa@northpole.com>
(version=TLS1_2 cipher=ECDHE-RSA-AES128-GCM-SHA256 bits=128/128);
Thu, 16 Mar 2017 04:44:28 -0700 (PDT)
Received-SPF: pass (google.com: domain of llbean.511020444@envfrm.r1234s5.comdesignates 199.8.123.38 as permitted sender) client-ip=199.8.123.38;
Authentication-Results: mx.google.com;
dkim=pass header.i=@e1.llbean.com;
dkim=pass header.i=@responsys.net;
dkim=pass header.i=@responsys.net;
spf=pass (google.com: domain of llbean.511020444@envfrm.r1234s5.comdesignates 199.8.123.38 as permitted sender) smtp.mailfrom=llbean.511020444@envfrm.r1234s5.com;
dmarc=pass (p=REJECT sp=REJECT dis=NONE) header.from=e1.llbean.com
Received: by omp.e1.llbean.com id hp9t9o1hctok for <santa@northpole.com>; Thu, 16 Mar 2017 04:25:51 -0700 (envelope-from <llbean.511020444@envfrm.r1234s5.com>)
X-CSA-Complaints: whitelist-complaints@eco.de
Received: by omp.e1.llbean.com id hp9r3u1hctou for <santa@northpole.com>; Thu, 16 Mar 2017 04:22:00 -0700 (envelope-from <llbean.50094@envfrm.rsys5.com>)
MIME-Version: 1.0
Content-Type: multipart/mixed; boundary="----msg_border_dPN32EhfaN"
Date: Thu, 16 Mar 2017 04:22:00 -0700
To: santa@northpole.com
From: "L.L.Bean" <llbean@e1.llbean.com>
Reply-To: "L.L.Bean" <reply@e1.llbean.com>
Subject: Climbing mountains. Breaking barriers.
Feedback-ID: 50021:10567834:oraclersys
Message-ID: <0.0.4D.537.1D2EDF347E56956.0@omp.e1.llbean.com>
------msg_border_dPN32EhfaN
Date: Thu, 16 Mar 2017 04:22:00 -0700
Content-Type: multipart/alternative; boundary="----alt_border_BrDQaPYzhN_1"
------alt_border_BrDQaPYzhN_1
Content-Type: text/plain; charset="UTF-8"
Content-Transfer-Encoding: quoted-printable
L.L.Bean Email Update
____________________________________________________________
To view this email with images, click below.

OutPut for Servlet service,


{
    "To": "santa@northpole.com",
    "From": "\"L.L.Bean\" <llbean@e1.llbean.com>",
    "Date": "Thu Mar 16 11:22:00 GMT 2017",
    "Subject": "Climbing mountains. Breaking barriers.",
    "Message-ID": "<0.0.4D.537.1D2EDF347E56956.0@omp.e1.llbean.com>\r\n "
}
