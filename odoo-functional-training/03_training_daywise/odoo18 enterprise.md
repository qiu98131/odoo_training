> odoo17企业版破解笔记（20250930）

共修改5处：

1、将\odoo17-e-企业版\odoo_updated_modules17\mail\models\update.py第108行到114行注释失效，添加一行pass，其余不变：完整如下：

```py
            # set_param = self.env['ir.config_parameter'].sudo().set_param
            # set_param('database.expiration_date', result['enterprise_info'].get('expiration_date'))
            # set_param('database.expiration_reason', result['enterprise_info'].get('expiration_reason', 'trial'))
            # set_param('database.enterprise_code', result['enterprise_info'].get('enterprise_code'))
            # set_param('database.already_linked_subscription_url', result['enterprise_info'].get('database_already_linked_subscription_url'))
            # set_param('database.already_linked_email', result['enterprise_info'].get('database_already_linked_email'))
            # set_param('database.already_linked_send_mail_url', result['enterprise_info'].get('database_already_linked_send_mail_url'))
            pass
```
此处完毕。

2、将\odoo17-e-企业版\odoo_updated_modules17\web\static\src\webclient\settings_form_view\widgets\res_config_edition.js第22行到27行的逻辑修改，完整如下：
将：

```js
   setup() {
    this.serverVersion = session.server_version;
    this.expirationDate = session.expiration_date
        ? DateTime.fromSQL(session.expiration_date).toLocaleString(DateTime.DATE_FULL)
        : DateTime.now().plus({ days: 30 }).toLocaleString(DateTime.DATE_FULL);
}
```

