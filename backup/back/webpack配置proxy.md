## webpack ����proxy �������

��ǰ�˿����ڼ� ,ǰ���ڱ������˷��񣬺ͺ�˵ķ����Ƕ���������

Ҫ���ʺ�˷�����漰���������⡣

����ǰ�˻�����`http://localhost:8090`,��̨�����ǣ�`http://localhost:80/simi`��ֱ�ӷ��ʻᱨ�������⡣


### ����webpack devServer �� proxyѡ��

    proxy: {
             '/index.php': {
    			target: 'http://localhost:80/simi',
                 changeOrigin: true,
                 secure: false
             }
             }

��������,��ʹ��axios��������ʱ��,ֻҪ��ǰ���`/index.php`��·��,���Զ���ת��`http://localhost:80/simi`������

�൱����ǰ��׷������ 'http://localhost:80/simi'

���ҿ��Կ���

---
���˼���,���ڿ�������������...

**ע��·������д�ظ�**,���ǰ�target **׷��** ����ǰ��!

