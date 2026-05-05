---
source-git-commit: 7abea6614a5c817cef3f83b293fab98974d4b072
workflow-type: tm+mt
source-wordcount: '63'
ht-degree: 0%

---
# PHP-tillägg för Cloud

<table style="table-layout:auto">
    <thead>
      <tr>
        <th>
            Standardtillägg
        </th>
        <th>
            Installerade tillägg som inte kan avinstalleras
        </th>
        <th>
            Tillägg som kan installeras och avinstalleras efter behov
        </th>
      </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <code>bcmath</code><br>
                <code>bz2</code><br>
                <code>calendar</code><br>
                <code>exif</code><br>
                <code>gd</code><br>
                <code>gettext</code><br>
                <code>intl</code><br>
                <code>libxml</code><br>
                <code>mysqli</code><br>
                <code>pcntl</code><br>
                <code>pdo_mysql</code><br>
                <code>Reflection</code><br>
                <code>soap</code><br>
                <code>sockets</code><br>
                <code>SPL</code><br>
                <code>standard</code><br>
                <code>swoole</code><br>
                <code>sysvmsg</code><br>
                <code>sysvsem</code><br>
                <code>sysvshm</code><br>
                <code>zip</code><br>
                <code>zlib</code><br>
            </td>
            <td>
                <code>ctype</code><br>
                <code>curl</code><br>
                <code>date</code><br>
                <code>dba</code><br>
                <code>dom</code><br>
                <code>fileinfo</code><br>
                <code>filter</code><br>
                <code>ftp</code><br>
                <code>hash</code><br>
                <code>iconv</code><br>
                <code>json</code><br>
                <code>mbstring</code><br>
                <code>mysqlnd</code><br>
                <code>openssl</code><br>
                <code>pcre</code><br>
                <code>pdo</code><br>
                <code>pdo_sqlite</code><br>
                <code>phar</code><br>
                <code>posix</code><br>
                <code>readline</code><br>
                <code>session</code><br>
                <code>sqlite3</code><br>
                <code>tokenizer</code><br>
                <code>xml</code><br>
                <code>xmlreader</code><br>
                <code>xmlwriter</code><br>
            </td>
            <td>
                <code>igbinary</code><br>
                <code>imap</code><br>
                <code>ldap</code><br>
                <code>mcrypt</code><br>
                <code>mysqli</code><br>
                <code>pdo_mysql</code><br>
                <code>propro</code><br>
                <code>recode</code><br>
                <code>redis</code><br>
                <code>shmop sockets</code><br>
                <code>sodium</code><br>
                <code>xmlrpc</code><br>
                <code>xsl</code><br>
            </td>
        </tr>
    </tbody>
</table>

>[!NOTE]
>
>Vissa PHP-tillägg har miljöspecifika installationsbegränsningar och visas inte helt i tabellen ovan. [!DNL LDAP] kan till exempel aktiveras i integreringsmiljöer via projektkonfiguration, men det är inte en självbetjäningskonfiguration för Pro Staging och Production via `.magento.app.yaml`.
