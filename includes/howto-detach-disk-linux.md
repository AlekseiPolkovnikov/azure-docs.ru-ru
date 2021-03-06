Когда диск данных, подключенный к виртуальной машине, больше не нужен, его можно легко отключить. При отсоединении диска от виртуальной машины он не удаляется из хранилища. Если вы хотите снова использовать существующие данные на диске, его можно легко повторно подключить как к той же самой, так и к другой виртуальной машине.  

> [!NOTE]
> Виртуальная машина в Azure использует различные типы дисков, такие как диск с операционной системой, локальный временный диск и дополнительные диски данных. Дополнительные сведения см. в статье [О дисках и виртуальных жестких дисках для виртуальных машин Azure](../articles/virtual-machines/virtual-machines-linux-about-disks-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Диск операционной системы отключить невозможно, если при этом вы не удаляете виртуальную машину.

## <a name="find-the-disk"></a>Поиск диска
Прежде чем отключить диск от виртуальной машины, необходимо узнать номер LUN (идентификатор диска, который необходимо отключить). Для этого выполните следующие действия:

1. Откройте Azure CLI и [подключитесь к подписке Azure](../articles/xplat-cli-connect.md). Убедитесь, что вы находитесь в режиме управления службами Azure (`azure config mode asm`).
2. Узнайте, какие диски подключены к вашей виртуальной машине. В следующем примере перечислены диски для виртуальной машины с именем `myVM`.

    ```azurecli
    azure vm disk list myVM
    ```

    Вы должны увидеть результат, аналогичный приведенному ниже.

    ```azurecli
    * Fetching disk images
    * Getting virtual machines
    * Getting VM disks
      data:    Lun  Size(GB)  Blob-Name                         OS
      data:    ---  --------  --------------------------------  -----
      data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
      data:    0    30        myDataDisk.vhd
      info:    vm disk list command OK
    ```

3. Запишите номер LUN ( **логический номер устройства** ) диска, который необходимо отсоединить.

## <a name="remove-operating-system-references-to-the-disk"></a>Удаление ссылок операционной системы на диск
Перед отключением диска от гостевой виртуальной машины Linux следует убедиться, что на этом диске не используется какие-либо разделы. Убедитесь, что операционная система не пытается повторно подключить их после перезагрузки. Выполнив эти инструкции, вы отмените конфигурацию, созданную при [подключении](../articles/virtual-machines/virtual-machines-linux-classic-attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) диска.

1. Используйте команду `lsscsi` , чтобы узнать идентификатор устройства. Вы можете установить `lsscsi` с помощью `yum install lsscsi` (в дистрибутивах на основе Red Hat) или `apt-get install lsscsi` (в дистрибутивах на основе Debian). Вы можете также найти идентификатор диска, используя приведенный выше логический номер устройства (LUN). Последний номер в кортеже в каждой строке и есть номер LUN. В следующем примере `lsscsi` используется, чтобы сопоставить LUN 0 с */dev/sdc*.

    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```

2. Используйте команду `fdisk -l <disk>`, чтобы найти разделы, связанные с диском, который должен быть отключен. В следующем примере показаны выходные данные для `/dev/sdc`.

    ```bash
    Disk /dev/sdc: 1098.4 GB, 1098437885952 bytes, 2145386496 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk label type: dos
    Disk identifier: 0x5a1d2a1a
    
        Device Boot      Start         End      Blocks   Id  System
    /dev/sdc1            2048  2145386495  1072692224   83  Linux
    ```

3. Отключите каждый раздел, указанный для диска. В следующем примере выполняется отключение `/dev/sdc1`.

    ```bash
    sudo umount /dev/sdc1
    ```

4. Используйте команду `blkid`, чтобы найти UUID для всех разделов. Вы должны увидеть результат, аналогичный приведенному ниже.

    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

5. Удалите записи из файла **/etc/fstab** , связанные с путями к устройству или идентификаторами UUID для всех разделов диска, который должен быть отключен.  Записи в этом примере могут быть такими:

    ```sh  
   UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2
   ```

    или
   
   ```sh   
   /dev/sdc1   /datadrive   ext4   defaults   1   2
   ```

## <a name="detach-the-disk"></a>Отсоединить диск
Когда вы найдете номер LUN диска и удалите ссылки операционной системы, диск можно будет отключить.

1. Отключите выбранный диск от виртуальной машины, выполнив команду `azure vm disk detach
   <virtual-machine-name> <LUN>`. В следующем примере LUN `0` отключается от виртуальной машины с именем `myVM`.
   
    ```azurecli
    azure vm disk detach myVM 0
    ```

2. Проверьте, отсоединен ли диск, повторно выполнив команду `azure vm disk list`. В следующем примере проверяется виртуальная машина с именем `myVM`.
   
    ```azurecli
    azure vm disk list myVM
    ```

    В результате будут выведены выходные данные, как в примере ниже, указывающие, что диск данных больше не присоединен.

    ```azurecli
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
     info:    vm disk list command OK
    ```

Отсоединенный диск остается в хранилище, но более не присоединен к виртуальной машине.



<!--HONumber=Nov16_HO3-->


