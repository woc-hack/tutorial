# WoC Hardware

|hostame|CPU|RAM|HDD|SSD|
|--|--|--|--|--|
|da[0-2]| Intel(R) Xeon(R) CPU E5-2630 v2 @ 2.60GHz - 24 cores | 396GB RAM | 35T HDD||
|da3| Intel(R) Xeon(R) CPU E5-2623 v3 @ 3.00GHz - 8 cores | 396GB RAM | 70T HDD |15TBSDD|
|da4|Intel(R) Xeon(R) CPU E5-2623 v4 @ 2.60GHz - 16 cores |792GB RAM |90T HDD |15TBSDD|
|da5|1Intel(R) Xeon(R) Gold 6148 CPU @ 2.40GHz - 80 cores|1.320TB RAM|124T HDD | 48TB SDD|
|da6|Intel(R) Xeon(R) Gold 6148 CPU @ 2.40GHz - 40 cores + 4 Nvidia Tesla V100-SXM2-32GB|256GiB RAM||1.9TSSD|
|bb0[^1]|||305TB HDD||
|bb1[^1]|||471TB HDD||
|da7[^1]|Intel(R) Xeon(R) Silver 4215R CPU @ 3.20GHz x2= 32 HW threads|384GiB RAM|655TB HDD ZFS ~550TB usable in 45-wide raidz2||
|da8[^1]|Intel(R) Xeon(R) Silver 4215R CPU @ 3.20GHz x2= 32 HW threads|384GiB RAM|655TB HDD ZFS ~510TB usable in 3x15-wide raidz2||

[^1]: These systems are primarily for storage and NOT recommended for running jobs. Access may not be available to all users, and they do not use the same mount points.
