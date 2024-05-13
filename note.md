install component:
gcloud components install gke-gcloud-auth-plugin


helm upgrade --install app --set image.repository=minhsphuc12/text-image-retrieval-serving --set image.tag=v1.5 .

edit file to update ssh key: 
/Users/phucnm/document_backup_m1/git/misc/continuous-deployment-to-gke-cluster/ansible/inventory 


openssl genpkey -algorithm RSA -out project_gke_img_retrieval.pem -pkeyopt rsa_keygen_bits:2048

openssl req -new -x509 -key project_gke_img_retrieval.pem -out certificate.pem -days 365

openssl rsa -pubout -in project_gke_img_retrieval.pem -out project_gke_img_retrieval_pub.pem

openssl pkey -in project_gke_img_retrieval.pem -text
openssl rsa -pubin -in project_gke_img_retrieval_pub.pem -text -noout

ssh -i /Users/phucnm/.ssh/project_gcp phucnm@35.193.158.171

sudo docker exec -ti jenkins bash
cat /var/jenkins_home/secrets/initialAdminPassword

export SA=jenkins
export PROJECT_ID=txt-img-retrieval
export SA_EMAIL=${SA}@${PROJECT_ID}.iam.gserviceaccount.com
gcloud iam service-accounts create $SA

gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member serviceAccount:$SA_EMAIL \
  --role roles/source.writer

gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member serviceAccount:$SA_EMAIL \
  --role roles/container.developer

  
