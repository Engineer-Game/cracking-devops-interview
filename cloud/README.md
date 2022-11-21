# Cloud

## **AWS**

### Ephemeral storage

Ephemeral disk is a temporary storage that it is added to your instance, and depending on your instance type the bigger is such storage. Please find a list of storage size available to you in the link below:

For some instances like ‘c1.medium’ and ‘m1.small’ they use instance storage automatically as SWAP as they have a limited amount of memory, while many others are automatically formatted and mounted at ‘/mnt’.

For instance, as they are a temporary storage, you should not rely on these disks to keep long-term data or even other data that you would not like to lose when a failure happens (i.e stop/start instance, failure on the underlying hardware, terminating instance), for these purposes please bear in mind that EBS/S3 or any other persistent storage will be your best choice.

### Elastic IP vs Public IP

When you launch an EC2 instance, you recieve a Public IP address by which that instance is reachable.

Once you stop that instance and restart the you get a new Public IP for the same instance's.

So, Public IP get's changed everytime for an instance after stop/start.

To overcome with this problem, we attach an Elastic IP to an Instance which doesn't change after you stop / start the instance as many times.

### Check limits in AWS

[http://awslimitchecker.readthedocs.io/en/latest/cli\_usage.html](http://awslimitchecker.readthedocs.io/en/latest/cli\_usage.html)

```
± |master ✓| → awslimitchecker -r us-east-1
awslimitchecker 0.7.0 is AGPL-licensed free software; all users have a right to the full source code of this version. See <https://github.com/jantman/awslimitchecker>
EBS/Active volumes                                     (limit 5000) CRITICAL: 5265
EBS/General Purpose (SSD) volume storage (GiB)         (limit 20480) CRITICAL: 240131
EBS/Magnetic volume storage (GiB)                      (limit 20480) CRITICAL: 35607
EC2/Running On-Demand c3.2xlarge instances             (limit 20) CRITICAL: 23
EC2/Running On-Demand c3.8xlarge instances             (limit 20) CRITICAL: 153
EC2/Running On-Demand c3.large instances               (limit 20) CRITICAL: 30
EC2/Running On-Demand d2.2xlarge instances             (limit 20) CRITICAL: 70
EC2/Running On-Demand d2.4xlarge instances             (limit 10) CRITICAL: 74
EC2/Running On-Demand d2.8xlarge instances             (limit 5) CRITICAL: 217
EC2/Running On-Demand d2.xlarge instances              (limit 20) CRITICAL: 49
EC2/Running On-Demand m3.large instances               (limit 20) CRITICAL: 259
EC2/Security groups per VPC                            (limit 500) CRITICAL: vpc-4df91828=794
EC2/VPC Elastic IP addresses (EIPs)                    (limit 3500) WARNING: 3178
EC2/VPC security groups per elastic network interface  (limit 5) CRITICAL: eni-910e2dcc=5, eni-e92f911a=5, eni-95fd4950=5, eni-26ac1ac7=5, eni-ce5941b8=5, eni-9d1b547c=5, eni-66f94da3=5, eni-180166f8=5, eni-095b89e8=5, eni-f81727b9=5, eni-006c315d=5, eni-41750c85=5, eni-930e2dce=5, eni-ffe6f1d1=5, eni-3264da6c=5, eni-8aa2146b=5, eni-24365fd6=5, eni-0e6f502d=5, eni-7fb265ba=5, eni-a8dae086=5, eni-672f9194=5, eni-b4a37947=5, eni-6852ec36=5, eni-9ebb0e7f=5, eni-cce78309=5, eni-bdbdd1cb=5, eni-a9378a5a=5, eni-092a94fa=5, eni-abdae085=5, eni-271e93d5=5, eni-2643d6c6=5, eni-6cb6d947=5, eni-02be2dc6=5, eni-6ddc698c=5, eni-984ed76a=5, eni-9873cc78=5, eni-aae18c59=5, eni-34be991b=5, eni-ab674ee4=5, eni-9ee4505b=5, eni-6078bd3e=5, eni-b520859e=5, eni-c113e5b7=5, eni-8c306e60=5, eni-f3e2a8b9=5, eni-817f56ce=5, eni-4054eda0=5, eni-ee87a0c1=5, eni-8c540ec2=5, eni-e9fe4a2c=5, eni-980e2dc5=5, eni-f2e2a8b8=5, eni-336a05f7=5, eni-fd27990e=5, eni-ee622fa5=5, eni-175fe149=5, eni-db228a1e=5, eni-8ba0166a=5, eni-8bae346a=5, eni-9aa5c7d5=5, eni-a0a59efa=5, eni-8ea812d0=5, eni-77d64b66=5, eni-422ba4b0=5, eni-c91eb60c=5, eni-458818b6=5 WARNING: eni-22b8517f=4, eni-bbe8e493=4, eni-a85dc1e2=4, eni-764e262b=4, eni-c4c384ea=4, eni-2822b476=4, eni-d50cbf8b=4, eni-46eeee68=4, eni-3f6b0866=4, eni-3cca9261=4, eni-c72fdce5=4, eni-3ee47edf=4, eni-a741f2f9=4, eni-9f30a75b=4, eni-68237b44=4, eni-6d52e133=4, eni-60d8bc80=4, eni-9619c5b4=4, eni-67cc2c39=4, eni-7db4489c=4, eni-cccd8420=4, eni-658adf4a=4, eni-5e139f11=4, eni-e8f44109=4, eni-a5c07afc=4, eni-f65d1bbc=4, eni-de916c8b=4, eni-dd1b73cd=4, eni-a5f0a5b5=4, eni-864c14db=4, eni-cfaa0225=4, eni-2f37a50e=4, eni-ec7d17fd=4, eni-64658eaa=4, eni-b7bdd1c1=4, eni-d87fd71d=4, eni-f2bf2baf=4, eni-295084ed=4, eni-73677193=4, eni-f9d496a3=4, eni-9c454e8d=4, eni-d66d69d0=4, eni-2cb8f502=4, eni-80b33193=4, eni-28ca9275=4, eni-4e471613=4, eni-1fb65245=4, eni-f3670bae=4, eni-77fc982f=4, eni-4e87ce13=4, eni-1fa6fa57=4, eni-7af8e15c=4, eni-96f6a6dc=4, eni-784e1625=4, eni-37da6c25=4, eni-0dca9250=4, eni-7b922e9b=4, eni-a0639840=4, eni-06f7942a=4, eni-a40ce78b=4, eni-ca12a70f=4, eni-10b6524a=4, eni-bc026ee1=4, eni-13237b3f=4, eni-625eed3c=4, eni-19a860e5=4, eni-12a6fa5a=4, eni-96333792=4, eni-6b14c87a=4, eni-a5374981=4, eni-9dbb53c8=4, eni-124296d6=4, eni-186da3ea=4, eni-f6a7dcf2=4, eni-122edd30=4, eni-8fe688f8=4, eni-fd5420a4=4, eni-e3c53df2=4, eni-5b7e3606=4, eni-844c14d9=4, eni-28034d2e=4, eni-6ba58524=4, eni-045c0227=4, eni-8e670bd3=4, eni-008340ee=4, eni-eaa351c8=4, eni-601c3621=4, eni-5dab3717=4, eni-e4f37d24=4, eni-282eb307=4, eni-63393a4d=4, eni-9f7ed65a=4, eni-5bffde19=4, eni-bf0b5faf=4, eni-de93cc93=4, eni-61dfe616=4, eni-fcb393b3=4, eni-f139d334=4, eni-14c244d7=4, eni-94baa357=4, eni-c29e85ed=4, eni-e5bdb4c8=4, eni-eaf74d1f=4, eni-2996a33b=4, eni-a23bc88f=4, eni-88670bd5=4, eni-4a89c112=4, eni-8a9648b3=4, eni-5f718abf=4, eni-23de68c2=4, eni-f026d5d2=4, eni-dd615180=4, eni-1e42561a=4, eni-dd197580=4, eni-956ad466=4, eni-4d5b95bf=4, eni-dec9c83f=4, eni-ebe95f0a=4, eni-085c88cc=4, eni-4d89c115=4, eni-5dcd2501=4, eni-c642f198=4, eni-bf3bc892=4, eni-53ddea7d=4, eni-621b773f=4, eni-bf026ee2=4, eni-b0028fec=4, eni-152ac834=4, eni-ae44eb89=4, eni-39679c75=4, eni-055a94f7=4, eni-a91c70f4=4, eni-ae6762e5=4, eni-8c37f97e=4, eni-ddca97c0=4, eni-f38559ef=4, eni-b6af5d94=4, eni-894c14d4=4, eni-3e659ede=4, eni-8d47dbc7=4, eni-785ce28b=4, eni-a441f2fa=4, eni-c375b5e5=4, eni-81b9d472=4, eni-0f03f02d=4, eni-721f7b60=4, eni-91a6eede=4, eni-bc16a379=4, eni-dc197581=4, eni-6f2ac84e=4, eni-16fe4c48=4, eni-11215bef=4, eni-577bfeb4=4, eni-d7b92d8a=4, eni-7b393a55=4, eni-353c25c7=4, eni-957f37c8=4, eni-c2e43292=4, eni-165858f7=4, eni-39ad8d76=4, eni-dd078c1d=4, eni-1cb65246=4, eni-32893a44=4, eni-f7bd57ae=4, eni-03038e5f=4, eni-807f37dd=4, eni-9f08eab2=4, eni-0f7fb72b=4, eni-3470d8f1=4, eni-f56c1de8=4, eni-8e8191c1=4, eni-7f075a2e=4, eni-6d31db8d=4, eni-1d506e0d=4, eni-b9b7459b=4, eni-8fc7c3d7=4, eni-ba3db8e8=4, eni-77905c50=4, eni-5b1151b4=4, eni-767edc6b=4, eni-65cc10a0=4, eni-c3c14dc6=4, eni-96da6c77=4, eni-cc64a1e0=4, eni-fa5d1bb0=4, eni-4234348c=4, eni-f364a1df=4, eni-d54277d3=4, eni-defff397=4, eni-119b074a=4, eni-eb77f519=4, eni-351701c7=4, eni-5a5e4475=4, eni-1e4ad654=4, eni-9c46ddd6=4, eni-b7b0fd99=4, eni-eaebeb24=4, eni-674227a7=4, eni-03a6fa4b=4, eni-1496cb56=4, eni-72b5953d=4, eni-4d603307=4, eni-737e362e=4, eni-12237b3e=4, eni-f6c6f1bc=4, eni-9e6905c3=4, eni-75b7973a=4, eni-54619ab4=4, eni-485f1902=4, eni-d97bd6fb=4, eni-d64fa18b=4, eni-6a237b46=4, eni-4599d018=4, eni-47c45d80=4, eni-5986a478=4, eni-65417948=4, eni-0e53744c=4, eni-beb410e0=4, eni-5c85b210=4, eni-4cae298f=4, eni-3e7f817e=4, eni-e69a51ae=4, eni-0343f05d=4, eni-99299c5c=4, eni-b708ea9a=4, eni-25c89078=4, eni-6efc9836=4, eni-e1f44100=4, eni-1db65247=4, eni-13b30549=4, eni-ba017aa9=4, eni-889e85a7=4, eni-81239644=4, eni-4f87ce12=4, eni-e426f220=4, eni-4c603306=4, eni-ecbf2bb1=4, eni-1117e433=4, eni-34a9897b=4, eni-a81c70f5=4, eni-79ddb687=4, eni-8aaeca4f=4, eni-2259e7d1=4, eni-afd0664e=4, eni-4141f21f=4, eni-c573dee7=4, eni-8164d7df=4, eni-a76905fa=4, eni-bb64984b=4, eni-8e97adc5=4, eni-82b688c9=4, eni-675be594=4, eni-13cfd0dd=4, eni-99eb19d4=4, eni-0b237b27=4, eni-70314791=4, eni-48204c16=4, eni-04d067e7=4, eni-6fdab49d=4, eni-1dcfbef2=4, eni-d056103e=4, eni-a069f25e=4, eni-1c90775c=4, eni-857de6cf=4, eni-94a33fde=4, eni-8fd23ccf=4, eni-6dce2e33=4, eni-15a6f104=4, eni-e1225b25=4, eni-de12e92d=4, eni-a0028ffc=4, eni-6b1b7736=4, eni-509fff1d=4, eni-ddd1b72c=4, eni-99dfffd6=4, eni-e99dc1eb=4, eni-36ec8868=4, eni-626be821=4, eni-2168d5d2=4, eni-6a9a8a25=4, eni-95b494da=4, eni-e16d9c26=4, eni-a9472ff4=4, eni-54ab371e=4
ELB/Active load balancers                              (limit 20) CRITICAL: 113
VPC/Internet gateways                                  (limit 5) CRITICAL: 5
VPC/VPCs                                               (limit 5) CRITICAL: 6
```

## **GCP**
