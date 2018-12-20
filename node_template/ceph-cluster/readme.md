�ڴ˴���compose�ļ�����������ceph�����£�
```
  ceph_exporter:
    container_name: ceph_exporter
    image: digitalocean/ceph_exporter:2.0.1-luminous
    network_mode: "host"
    volumes:
      - /etc/ceph:/etc/ceph
    ports:
      - 9128:9128
    labels:
      SERVICE_TAGS: ceph-cluster
    logging:
      driver: "json-file"
      options:
        max-size: "1G"
```

�����Ҳ��Ҫ�������ļ��޸�
```
  - job_name: 'ceph_exporter'
    metrics_path: /metrics
    scheme: http
    consul_sd_configs:
      - server: 127.0.0.1:8500
        services: ['ceph_exporter']
    relabel_configs:
        - source_labels: ['__meta_consul_service']
          regex:         '(.*)'
          target_label:  'job'
          replacement:   '$1'
        - source_labels: ['__meta_consul_service_address']
          regex:         '(.*)'
          target_label:  'instance'
          replacement:   '$1'
        - source_labels: ['__meta_consul_service_address', '__meta_consul_service_port']
          regex:         '(.*);(.*)'
          target_label:  '__address__'
          replacement:   '$1:$2'

        - source_labels: ['__meta_consul_tags']
          regex:         ',(ceph-cluster|cephfs),'
          target_label:  'group'
          replacement:   '$1'
```
����ceph-cluster|cephfs�������ı�ǩ��Ҳ��Ӧ�˴���ģ��
