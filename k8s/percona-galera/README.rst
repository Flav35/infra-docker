####################
percona-galera chart
####################

To use this chart :

.. code-block:: bash

   $ git clone git@gitlab.objectif-libre.com:ol/k8s/charts.git
   $ cd charts/percona-galera
   $ helm install .

*root password* and *sstuser password* are automatically generate. If you want to choose them you have to provide a value for *mysqlRootPassword* or *sstPassword*
