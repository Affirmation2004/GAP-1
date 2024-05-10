# GAP-1
Устанавливаем Rocky Linux 9

устанавливаем прометей

sudo dnf -y install epel-release vim
curl -s https://packagecloud.io/install/repositories/prometheus-rpm/release/script.rpm.sh | sudo bash
dnf install prometheus -y
vim /etc/prometheus/prometheus.yml
systemctl start prometheus
systemctl enable prometheus
systemctl status prometheus.service 

Ставим node_exporter

dnf -y install node_exporter
systemctl enable --now node_exporter
systemctl status node_exporter
firewall-cmd --add-port=9100/tcp --permanent
sudo firewall-cmd --reload

Настраиваем prometheus

vim /etc/prometheus/prometheus.yml
В разделе Scrape_config добавляем

  - job_name: 'node_exporter_metrics'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9100']

systemctl restart prometheus

Ставим blackbox_exporter

dnf -y install blackbox_exporter
systemctl enable --now blackbox_exporter
sudo firewall-cmd --add-port=9115/tcp --permanent
sudo firewall-cmd --reload
sudo systemctl daemon-reload
sudo systemctl enable --now blackbox_exporter
systemctl status blackbox_exporter
firewall-cmd --add-port=9115/tcp --permanent
firewall-cmd --reload
