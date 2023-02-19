# vagrant-k8s-cluster-initializer
Create automaticaly k8s cluster with 3 nodes and making dashboard using Kubeadm and Vagrant

```
> vagrant up 
> vagrant ssh master
> kubectl top nodes
> kubectl get po -n kube-system
> mkdir -p $HOME/.kube
> sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
> sudo chown $(id -u):$(id -g) $HOME/.kube/config
> export KUBECONFIG=/etc/kubernetes/admin.conf
> kubectl cluster-info
```

node'lar üzerinde yapılan işlemler
```
> kubeadm join <ip_address:6443> --token <master_node_üzerinden_alınan_token>
> kubeadm token list
> openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt |    openssl rsa -pubin -outform der 2>/dev/null |    openssl dgst -sha256 -hex | sed 's/^.* //'
```

# Dashboard kurulumu
```
> kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.6.1/aio/deploy/recommended.yaml
> kubectl get all -n kubernetes-dashboard
> kubectl apply -f dashboardServiceAccount.yaml
> kubectl apply -f clusterRoleBindingDashboard.yaml
> kubectl -n kubernetes-dashboard create token demo-user-admin
> kubectl patch svc kubernetes-dashboard --type='json' -p '[{"op":"replace","path":"/spec/type","value":"NodePort"}]'
```

Virtual machine'de çalıştırıldığı için Ram veya Cpu yetmediği durumda CrashLoopBackoff hatası alınabilir. Pod'un yeniden çalıştırılması lazım veya donanımsal özelliklerin yükseltilmesi. Export ile o session için komutlar çalıştırılır. Export komutları vagrant her açıldığında uygulanabilir veya sistemin yeniden çalıştırılmasına uygun hale entegre edilebilir.
