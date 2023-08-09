What is storage class ?

A StorageClass in Kubernetes is a way to describe different "classes" of storage that are available within the Kubernetes cluster. It's used to define how a unit of storage, a PersistentVolume, should be provisioned.

For example, you might have some fast SSD storage available on some nodes, and some slower, but cheaper HDD storage available on others. You could define two StorageClasses - one for each type - and then when users create a PersistentVolumeClaim (a request for storage), they can specify which class they would like to use.

Each StorageClass contains the fields provisioner, parameters, and reclaimPolicy, which are used when a PersistentVolume belonging to the class needs to be dynamically provisioned.

Here are a few key elements involved:

Provisioner: It determines what volume plugin is used for provisioning PVs. Some common provisioners include aws-ebs, gce-pd, glusterfs, iscsi, azureDisk, azureFile, etc.

Parameters: These are used to configure the provisioner. The parameters field contains key-value pairs where the provisioner specifies the options for the volume. The key-value pairs depend on the provisioner being used.

ReclaimPolicy: It determines what happens to a volume when a user is done with it. Possible reclaim policies are Retain, Delete, and Recycle. If no reclaimPolicy is specified when a StorageClass object is created, it will default to Delete.

VolumeBindingMode: It determines when volume binding and dynamic provisioning should occur. The default mode is Immediate, which means that the volume binding will occur immediately once the PVC is created.

Why do we need storage class ? StorageClass in Kubernetes serves an important role for managing storage resources in a Kubernetes cluster. It provides a way for administrators to describe the "classes" of storage they offer.

Here are a few reasons why StorageClass is needed in Kubernetes:

Abstraction: StorageClass provides an abstract layer between the storage volume and the application. The users or the applications do not need to know the underlying storage system. They just need to request the volume with the StorageClass.

Dynamic Volume Provisioning: With StorageClass, you can dynamically provision storage volumes. This means you don't have to pre-provision storage, but rather, storage volumes can be created on-demand as applications require them.

Diverse Storage Options: Kubernetes supports a wide range of storage systems. Different types of applications may have different storage needs. With StorageClass, you can create different classes for different types of storage - for example, SSD for high-speed storage, or HDD for cheaper, slower storage.

Storage Configuration: StorageClass allows you to define how the storage should be provisioned and what parameters to apply, like type of disk, IOPS, and more. Different StorageClasses can have different configurations as per the application's needs.

Efficient Resource Utilization: By allowing dynamic provisioning and specific configurations, you can ensure that resources are used efficiently, and you only provision the storage that you need.

In short, StorageClass helps to manage and allocate storage based on the quality of service required by applications, and provide an easy way to offer a variety of storage resources.

Here is an example of a StorageClass for AWS:

apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: slow
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
  fsType: ext4
In this example, a StorageClass named "slow" is created, which provisions AWS EBS volumes of type gp2 with a filesystem of type ext4.

Note that different cloud providers support different provisioners and parameters. You will need to refer to your specific provider's documentation to understand the specific options available.

What is Persistent Volume ?

A PersistentVolume (PV) in Kubernetes is a piece of networked storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes. It's a resource in the cluster just like a node is a cluster resource.

PVs are volume plugins like Volumes, but have a lifecycle independent of any individual Pod that uses the PV. This API object captures the details of the implementation of the storage, be that NFS, iSCSI, or a cloud-provider-specific storage system.

Here are some key points about PersistentVolumes:

Lifecycle: Unlike ephemeral local storage, which is tightly coupled to a single pod (a pod's life is identical to the volume's life), persistent volume lives independently of any Pods and persists beyond the lifecycle of any individual Pod.

Storage Size: Each PV has a specific size. This is set at the time the volume is created.

Access Modes: Each PV has a specific access mode - it can be ReadWriteOnce (can be mounted as read/write by a single node), ReadOnlyMany (can be mounted read-only by many nodes), or ReadWriteMany (can be mounted as read/write by many nodes).

Reclaim Policy: It tells the cluster what to do with the volume after it has been released of its claim. Currently, volumes can either be Retained, Recycled, or Deleted.

Mount Options: A PersistentVolume can specify mount options, which will be used when mounting the volume.

Volume Mode: It can have VolumeMode. VolumeModes are used to determine if a volume is a raw block device or a filesystem.

StorageClass: Each PV can have a StorageClass. The PV must belong to a storage class and this field must be set.

PVs can either be statically or dynamically provisioned. In static provisioning, the cluster administrator creates a number of PVs. They carry the details of the real storage, which is available for use by cluster users. In dynamic provisioning, the cluster administrator creates a number of StorageClasses. A user then creates a PersistentVolumeClaim with a certain storage size and access mode, and Kubernetes then creates the PV for them.

Overall, PersistentVolumes provide a way for users and administrators to abstract details of how storage is provided and how it is consumed.

Create a PV from storageclass?

To create a PersistentVolume (PV) using a StorageClass, you would actually create what's known as a PersistentVolumeClaim (PVC). This claim will either bind to an existing PV that matches the claim's requirements, or trigger dynamic provisioning to create a new PV if no matching existing PV is found and dynamic provisioning is enabled for the StorageClass.

Here is an example of how you might create a PVC that uses a specific StorageClass:

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 8Gi
  storageClassName: my-storage-class
In this example:

name is the name you want to assign to the PVC.

accessModes specifies the desired access mode. In this case, ReadWriteOnce means that the volume can be mounted as read-write by a single node.

resources: requests: storage specifies the amount of storage you want to request. In this case, we're requesting 8 gibibytes.

storageClassName specifies the name of the StorageClass that should be used to provision the PersistentVolume.

This YAML would be saved in a file (let's say pvc.yaml), and then you would run:

kubectl apply -f pvc.yaml
If the StorageClass my-storage-class is configured for dynamic provisioning and there's no existing PV that fulfills the claim's requirements, this command will create a PVC and a new PV will be dynamically provisioned based on the StorageClass. The PVC will automatically be bound to the new PV.

You can check the status of PVC using:

kubectl get pvc my-pvc
And you can also verify the created PV using:

kubectl get pv
What is the difference between pv and pvc ?

In Kubernetes, PersistentVolumes (PVs) and PersistentVolumeClaims (PVCs) are two different API objects that work together to provide long-term storage that outlives the lifecycle of individual Pods.

Here's how they differ:

PersistentVolume (PV): A PersistentVolume (PV) is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes. It is a resource just like a node that is cluster scoped, which means PV is available across all namespaces. It's a representation of a piece of actual storage capacity in the infrastructure, whether that's local storage on a physical machine or network-attached storage of some kind.

PersistentVolumeClaim (PVC): A PersistentVolumeClaim (PVC) is a request for storage by a user that can be fulfilled by a PersistentVolume. While PVs are resources in the cluster, PVCs are requests for those resources and also act as claim checks to the resource. Claims are a user's method of requesting a specific size, access modes, or StorageClass in their storage, and a way for a user to abstract the details of the storage. PVCs are namespaced scoped, which means they can only be referenced by Pods within the same namespace.

The relationship between PVs and PVCs is like the one between a Node and a Pod. Just as Pods consume node resources, PVCs consume PV resources. More specifically, PVCs bind to PVs either manually (PVs and PVCs have specific names) or dynamically (Kubernetes provisions the storage and automatically binds the PVC to the PV).

In short, PVs represent physical storage resources in the cluster, while PVCs represent requests for those resources and also act as a way to claim the use of a PV for a Pod.

How to create a PV in storageclass ?

If you want to access a storage class for creating a Persistent Volume (PV), you actually don't create the PV directly. Instead, you would create a Persistent Volume Claim (PVC), which can either bind to an existing PV that matches its requirements, or trigger dynamic provisioning to create a new PV, if no suitable PV is found and dynamic provisioning is enabled for the specified StorageClass.

Here is an example of a PVC YAML manifest that refers to a StorageClass:

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: my-storage-class
In this example:

name is the name you want to assign to the PVC.

accessModes specifies how the volume should be accessed. In this case, ReadWriteOnce means that the volume can be mounted as read-write by a single node.

resources: requests: storage is the amount of storage you want to request. In this case, we're requesting 1 gibibyte of storage.

storageClassName specifies the name of the StorageClass that should be used to provision the PV.

This YAML would be saved in a file (let's say pvc.yaml), and then you would run:

kubectl apply -f pvc.yaml
If dynamic provisioning is set up for the my-storage-class StorageClass and there's no existing PV that fulfills the claim's requirements, this command will create a PVC and a new PV will be dynamically provisioned based on the StorageClass. The PVC will automatically be bound to the new PV.

You can check the status of PVC using:

kubectl get pvc my-pvc
And you can also verify the created PV using:

kubectl get pv
The PV is automatically created and managed by Kubernetes when you create the PVC that refers to the StorageClass.

Creating PVC in StorageClass ?

To create a PersistentVolumeClaim (PVC) within a specific StorageClass, you need to create a YAML definition for the PVC that specifies the StorageClass. Here's an example:

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: my-storage-class
In this YAML file:

name is the name of the PVC.

accessModes is the access mode the PVC requires. ReadWriteOnce means that the volume can be mounted as read-write by a single node.

resources: requests: storage specifies the size of the storage that the PVC requests. Here, it's 1 gibibyte.

storageClassName is the name of the StorageClass from which the PVC requests a PersistentVolume. In this case, the PVC is requesting a PersistentVolume from the my-storage-class StorageClass.

You can save the above YAML in a file (say pvc.yaml) and then use the kubectl apply command to create the PVC in the Kubernetes cluster:

kubectl apply -f pvc.yaml
If a PersistentVolume that satisfies the PVC's requirements exists, the PVC will bind to that volume. If no such volume exists, and dynamic provisioning is set up for my-storage-class, Kubernetes will automatically provision a new volume for the PVC.

After creating the PVC, you can check its status with the following command:

kubectl get pvc my-pvc
You can also verify the corresponding PersistentVolume (if dynamically provisioned) using:

kubectl get pv
The PV will have a name automatically generated by Kubernetes.

Importing Volume to your Pod

If you want to use a PersistentVolumeClaim (PVC) in your Pod, you will have to define it in your Pod's YAML file. In your Pod's specification, you can reference the PVC under volumes, and then refer to that volume in the volumeMounts section of your Pod's container specification.

Here's an example:

apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: my-image
    volumeMounts:
    - name: my-volume
      mountPath: /path/to/mount
  volumes:
  - name: my-volume
    persistentVolumeClaim:
      claimName: my-pvc
In this example:

name in the volumeMounts section is the name you want to assign to the volume inside the container.

mountPath is the path in the container where the volume will be mounted.

In the volumes section, name is the name you want to assign to the volume in the Pod specification (this name must match the name given in the volumeMounts section).

persistentVolumeClaim: claimName is the name of the PersistentVolumeClaim that the Pod will use for the volume.

In this way, the Pod my-pod will use the PersistentVolume that is bound to the PersistentVolumeClaim my-pvc, and it will mount it at /path/to/mount in the container my-container.

Please note that the PVC my-pvc must exist in the same namespace as the Pod. The PVC and the Pod are namespaced resources and live in the same namespace.

LAB Exercise

Sure, here's a simple lab exercise on creating a PersistentVolume (PV), a PersistentVolumeClaim (PVC), and then using it in a Pod.

Exercise:

Create a PersistentVolume with the name my-pv, access mode ReadWriteOnce, and a capacity of 1Gi. Use the manual StorageClass and the hostPath volume type with the path /mnt/data.

Create a PersistentVolumeClaim with the name my-pvc that requests the storage with access mode ReadWriteOnce and storage request of 1Gi.

Create a Pod named mypod with an nginx container. Mount the volume from the PVC my-pvc at the path /usr/share/nginx/html in the container.

Solution:

PersistentVolume

Create a YAML file named my-pv.yaml:

apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 1Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: manual
  hostPath:
    path: "/mnt/data"
Apply it using kubectl:

kubectl apply -f my-pv.yaml
PersistentVolumeClaim

Create a YAML file named my-pvc.yaml:

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: manual
Apply it using kubectl:

kubectl apply -f my-pvc.yaml
Pod

Create a YAML file named mypod.yaml:

apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - mountPath: "/usr/share/nginx/html"
      name: mypd
  volumes:
  - name: mypd
    persistentVolumeClaim:
      claimName: my-pvc
Apply it using kubectl:

kubectl apply -f mypod.yaml
Verify

You can check the created PV, PVC, and Pod with the following commands:

kubectl get pv
kubectl get pvc
kubectl get pod
This exercise creates a PV and a PVC, and then it mounts the volume from the PVC into an nginx Pod. Note that the hostPath type used here is suitable only for development and testing. In a real-world cluster, you would probably use a networked storage type, such as those provided by cloud platforms.
