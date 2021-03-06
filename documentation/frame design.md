# Groupwork 3  

------------------------
### Frame type (all)

|  message meta type  | descriptor |
| :-----------------: | :--------: |
|        info         |    0x00    |
|    info_respond     |    0x01    |
|  passwd/new_passwd  |    0x02    |
|   passwd_respond    |    0x03    |
|       refuse        |    0x04    |
|      configure      |    0x05    |
|  history_user_name  |    0x06    |
|       history       |    0x07    |
| synchronization_end |    0x08    |
|      text_user      |    0x09    |
|        text         |    0x0A    |
|      file_name      |    0x0B    |
|        file         |    0x0C    |
| group_text_userlist |    0x0D    |



----------------------------
## Log In

### Frame type

| message meta type | descriptor |
| :---------------: | :--------: |
|       info        |    0x00    |
|   info_respond    |    0x01    |
| passwd/new_passwd |    0x02    |
|  passwd_respond   |    0x03    |
|      refuse       |    0x04    |

-----

#### info `0x00`
|  0   |         1 , 2         |     3 ... 31      |
| :--: | :-------------------: | :---------------: |
| 0x00 | info_length (2 bytes) | data（user_name） |



#### info_respond `0x01`

|  0   |      1       |
| :--: | :----------: |
| 0x01 | respond type |

 > **respond type:**
 > >|         0x0         |   0x1   |
 > >| :-----------------: | :-----: |
 > >| user does not exist | succeed |



#### passwd  `0x02`

|  0   |          1 , 2          |        3 ... 31         |
| :--: | :---------------------: | :---------------------: |
| 0x02 | passwd_length (2 bytes) | password (28 bytes max) |



#### passwd_respond `0x03`

|  0   |          1          |
| :--: | :-----------------: |
| 0x03 | passwd_respond type |

>  **passwd_respond type：**

> |   0x1   |      0x2      |  0x3  |
> | :-----: | :-----------: | :---: |
> | correct | change passwd | wrong |
> 密码错误client直接踢掉



#### new_passwd `0x02` 

|  0   |          1 , 2          |        3 ... 31         |
| :--: | :---------------------: | :---------------------: |
| 0x02 | passwd_length (2 bytes) | password (28 bytes max) |



#### refuse  `0x04`

|  0   |
| :--: |
| 0x04 |

-----------------------
## Synchronization



### Frame type

|     frame type      | descriptor |
| :-----------------: | :--------: |
|      configure      |    0x05    |
|  history_user_name  |    0x06    |
|       history       |    0x07    |
| synchronization_end |    0x08    |



#### configure  `0x05`
|  0   |          1 , 2          |
| :--: | :---------------------: |
| 0x05 | record_length (2 bytes) |



#### history_user_name  `0x06`

|  0   |            1, 2            | 3 ... 31  |
| :--: | :------------------------: | :-------: |
| 0x06 | user_name length (2 bytes) | user_name |



#### history  `0x07`

|  0   |     1, 2      | 3 ... record_length+3 |
| :--: | :-----------: | :-------------------: |
| 0x07 | record_length |        record         |



#### synchronization _end   `0x08`

1 byte

|  0   |
| :--: |
| 0x08 |





----------
## Communication



### Message meta

|  message meta type  | descriptor |
| :-----------------: | :--------: |
|      text_user      |    0x09    |
|      fil_name       |    0x0B    |
| group_text_userlist |    0x0F    |



### Sending message

#### text_user `0x09`

32 bytes

|  0   |           1 , 2            |         3 ... 31         |
| :--: | :------------------------: | :----------------------: |
| 0x09 | user_name_length (2 bytes) | user_name (28 bytes max) |



#### text `0x0A`

max : 1024 bytes


|  0   |          1 , 2           |  3 ... (message_length + 3)   |
| :--: | :----------------------: | :---------------------------: |
| 0x0A | message_length (2 bytes) | message_data (1021 bytes max) |



### Sending file

#### file_name `0x0B`
|  0   |           1 , 2            |         3 ... 31         |
| :--: | :------------------------: | :----------------------: |
| 0x0B | file_name_length (2 bytes) | file_name (28 bytes max) |



#### file `0x0C`

max : 1024 bytes

|  0   |                            1 , 2                             |   3 ... (file_length+3)    |
| :--: | :----------------------------------------------------------: | :------------------------: |
| 0x0C | file_length (2 bytes) (0 if remaining file_length > 1021 else file_length) | file_data (1021 bytes max) |



### Sending group message

#### group_text_userlist `0x0D`
1024 bytes

|  0   |              1, 2              |  3 ... 1024   |
| :--: | :----------------------------: | :-----------: |
| 0x0D | username_list_length (2 bytes) | username_list |

> username_list 包含最后一个username的 `'\0'`



#### message `0x0A`

max : 1024 bytes

|  0   |          1 , 2           |  3 ... (message_length + 3)   |
| :--: | :----------------------: | :---------------------------: |
| 0x0A | message_length (2 bytes) | message_data (1021 bytes max) |



