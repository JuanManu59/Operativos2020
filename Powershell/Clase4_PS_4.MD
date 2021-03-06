EJERCICIOS
1.	Mostrar una tabla de procesos que incluya �nicamente los nombres de los procesos, sus IDs, y si est�n respondiendo a Windows (la propiedad Responding muestra eso). Haga que la tabla tome el m�nimo de espacio horizontal, pero no permita que la informaci�n se trunque.

Get-Process | ft -Property Name, ID, Responding -AutoSize -Wrap

2.	Muestre una tabla de procesos que incluya los nombres de los procesos y sus IDs. Tambi�n incluya columnas para uso de memoria virtual y f�sica; exprese dichos valores en megabytes (MB).
Get-Process | ft -Property Name, ID, PagedMemorySize, PrivateMemorySize, VirtualMemorySize -AutoSize -Wrap
3.	Emplee Get-EventLog para mostrar una lista de los logs de eventos disponibles (revise la ayuda para encontrar el par�metro que le permitir� obtener dicha informaci�n). Formatee la salida como una tabla que incluya el nombre de despliegue del log y el per�odo de retenci�n. Los encabezados de columna deben ser NombreLog y Per-Retencion.

Get-EventLog -List | ft @{name='NombreLog';expression={$_.LogDisplayName}},@{name='Per-Retencion';expression={$_.MinimumRetentionDays}} 


4.	Muestre una lista de servicios, de tal manera que aparezcan agrupados los servicios que est�n iniciados y los que est�n detenidos. Los que est�n iniciados deben aparecer primero.
Get-Service | sort status -Descending
5.	Mostrar una lista a cuatro columnas de todos los directorios que est�n en el ra�z de la unidad C:

ls C:| format-wide name -col 4

6.	Cree una lista formateada de todos los archivos .exe del directorio C:\Windows. Debe mostrarse el nombre, la informaci�n de versi�n, y el tama�o del archivo. La propiedad de tama�o se llama length en Powershell, pero para mayor claridad, la columna se debe llamar Tama�o en su listado.


ls C:\Windows | where -filter { $_.Name -like '*.exe'} | fl Name,VersionInfo,@{n='Tama�o';e={$_.Length}} 

7.	Importe el m�dulo NetAdapter (empleando el comando Import-Module NetAdapter). Empleando el cmdlet Get-NetAdapter, muestre una lista de adaptadores no virtuales (adaptadores cuya propiedad Virtual sea falsa. El valor l�gico falso es representado por Powershell como $False).

Get-NetAdapter | where -Filter {$_.Virtual -eq $false} | fl

8.	Importe el m�dulo DnsClient. Empleando el cmdlet Get-DnsClientCache, muestre una lista de los registros A y AAAA que est�n en el cach�. Sugerencia: Si el cach� est� vac�o, visite algunos sitios web para poblarlo.

	Get-DnsClientCache | Where -Filter{$_.Type -eq 1 -or $_.Type -eq 28} | fl

9.	Genere una lista de todos los archivos .exe del directorio C:\Windows\System32 que tengan m�s de 5 MB.

ls C:\Windows\System32 | where -filter {$_.Name -like �*.exe� -and $_.Length -gt 5*1MB} | fl Name, VersionInfo, @{n=�Tama�o�; e= {$_.Length}}
10.	Muestre una lista de parches que sean actualizaciones de seguridad.
Get-HotFix | Where -Filter {$_.Description -eq �Security Update�} | fl
11.	Muestre una lista de parches que hayan sido instalados por el usuario Administrador, que sean actualizaciones. Si no tiene ninguno, busque parches instalados por el usuario System. Note que algunos parches no tienen valor en el campo Installed By.

Get-HotFix | Where -Filter {$_.Description -eq 'Update' -and $_.InstalledBy -eq 'NT AUTHORITY\SYSTEM'} | fl 

12.	Genere una lista de todos los procesos que est�n corriendo con el nombre Conhost o Svchost.
Get-Process | Where -Filter {$_.name -eq �Conhost� -or $_.Name -eq �Svchost�} | fl
