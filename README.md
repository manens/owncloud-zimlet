# Zimbra ownCloud Zimlet
==========

Add ownCloud or any WebDAV server to your Zimbra webmail.

If you find Zimbra ownCloud Zimlet useful and want to support its continued development, you can make donations via:
- PayPal: info@barrydegraaff.tk
- Bank transfer: IBAN NL55ABNA0623226413 ; BIC ABNANL2A

========================================================================


  - - - DO NOT INSTALL THIS ZIMLET AS IT IS NOT YET RELEASED - - -


========================================================================

### Installing

    su zimbra
    cd /tmp
    rm -f tk_barrydegraaff_owncloud_zimlet*
    wget https://github.com/barrydegraaff/owncloud-zimlet-binaries/raw/master/0.0.4/tk_barrydegraaff_owncloud_zimlet.zip
    zmzimletctl deploy tk_barrydegraaff_owncloud_zimlet.zip
    
Wait 15 minutes for the deploy to propagate; or run ```zmprov fc all && zmmailboxdctl restart```.
    
Modify the default COS to expand the Zimlets menu by default:

    zmprov mc default zimbraPrefZimletTreeOpen TRUE

To avoid JavaScript same origin policy problems we configure ownCloud or the WebDAV server of your choice to be inside the same domain as your Zimbra server.

    Add to the bottom of /opt/zimbra/conf/nginx/templates/nginx.conf.web.https.default.template before the final }
    location /owncloud/ {
        proxy_pass https://owncloud.example.com/owncloud/;
    }

Then as Zimbra user: ```zmproxyctl restart```

This will add ownCloud under the same domain as your Zimbra server: https://zimbraserver.example.com/owncloud/ 

In the ownCloud server, comment a line in the css so the Deleted Items menu becomes visible in Zimbra:

    /var/www/html/owncloud/apps/files/css/files.css
    .nav-trashbin {
    /*	position: fixed !important; */

========================================================================

### Installation of ownCloud in a custom location

If your ownCloud is configured in a location other than /owncloud for example /oc you will have to configure the global configuration:

    su zimbra
    zmzimletctl getConfigTemplate tk_barrydegraaff_owncloud_zimlet.zip > config_template.xml

Make changes to config_template.xml then:

    zmzimletctl configure config_template.xml 

Also you should alter the proxy configuration to match your location:

    Add to the bottom of /opt/zimbra/conf/nginx/templates/nginx.conf.web.https.default.template before the final }
    location /oc/ {
        proxy_pass https://owncloud.example.com/oc/;
    }


========================================================================

### License

Copyright (C) 2015  Barry de Graaff

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see http://www.gnu.org/licenses/.
