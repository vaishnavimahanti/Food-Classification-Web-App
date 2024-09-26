# Food_classification_web_application with VGG16 and Kubernetes

Introduction:

This repository demonstrates the use of transfer learning with the VGG16 convolutional neural network architecture, fine-tuned to classify various food categories. The model is deployed on a Kubernetes cluster, and its performance is monitored using resource tracking tools. Below are the key components and methodologies used to build, train, and deploy the model.

Model Overview:

The base model used in this project is VGG16, a convolutional neural network architecture pre-trained on the ImageNet dataset. The VGG16 model consists of 14,714,688 parameters.

Data Augmentation:

To improve model generalization and prevent overfitting, tf.keras.preprocessing.image.ImageDataGenerator is used to perform various data augmentation techniques:

Rescaling: Adjusts image pixel values to fall between 0 and 1.
Rotation: Randomly rotates the images by 1 degree.
Zooming: Random zooming by up to 10%.
Width and Height Shift: Randomly shifts the image width and height by 10%.
Shear Transformation: Randomly applies shear transformations within the range of 10%.

Training Process:

The classification head of the VGG16 model is trained with 20 epochs using the Adam optimizer, with a learning rate of 0.001 and a batch size of 32. This results in a validation accuracy of 84.55%. For fine-tuning, the last five layers of the VGG16 base model are unfrozen and trained again with a learning rate of 0.0001. The fine-tuning process yields a validation accuracy of 81.63%.

Final Evaluation

Final Accuracy on the evaluation set: 0.8476

Model Performance:

Memory Requested: 4194304 KB
Memory Used: 9773072 KB
Transaction Rate: 0.61
Response Time: 1.82 seconds
Accuracy: 0.84

Kubernetes Cluster Setup and Model Deployment:

The following steps outline the deployment of the machine learning model on a Kubernetes cluster:

SSH Key Generation: Use ssh-keygen -t rsa to generate public and private keys for secure access to cloud resources.
Kubernetes Cluster Setup: Inside the nyu-ece6143 project, an experiment is created, and the Kubernetes cluster is set up using a provided setup script.
Container Creation: A Docker container named ml-app is built and pushed to the local distribution registry. The container holds the machine learning model.
Model Transfer: Using the scp command, the model.keras file is transferred to the remote machine.
Deployment with Horizontal Pod Autoscaler (HPA): The deployment_hpa.yaml configuration is used to deploy the model with dynamic scaling.
Resource Monitoring: A Python script from the k8s-ml repository monitors resource utilization, logging data in CSV format.
Load Testing: The load_test.sh script is run to test system behavior under different workloads.
Extended Monitoring: A modified resource monitoring script is run over an extended period to collect further resource usage data.
Data Analysis: CSV files (load_output.csv and resource_usage.csv) are transferred back to the local machine using scp for analysis.
Model Termination: The deployed models are terminated using kubectl delete -f /k8s-ml/deploy_hpa/deployment_hpa.yaml.

Conclusion:

This project demonstrates a comprehensive approach to building and deploying a deep learning model using VGG16 for image classification and Kubernetes for scalable deployment. The use of transfer learning, data augmentation, and fine-tuning techniques, combined with effective resource monitoring, ensures the model performs optimally in real-world environments.
