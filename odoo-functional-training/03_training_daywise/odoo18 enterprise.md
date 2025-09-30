> # odoo17企业版破解笔记（20250930）

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
改为：

```js
   setup() {
    this.serverVersion = session.server_version;
    this.expirationDate = "This is an open version and has no expiration date."
            }
```
此处完毕。


3、将\odoo17-e-企业版\odoo_updated_modules17\web_enterprise\models\ir_http.py第23行到40行全部注释失效，完整如下：

```py
            # def session_info(self):
            #     ICP = self.env['ir.config_parameter'].sudo()
            #     User = self.env['res.users']
            
            #     if User.has_group('base.group_system'):
            #         warn_enterprise = 'admin'
            #     elif User.has_group('base.group_user'):
            #         warn_enterprise = 'user'
            #     else:
            #         warn_enterprise = False
            
            #     result = super(Http, self).session_info()
            #     result['support_url'] = "https://www.odoo.com/help"
            #     if warn_enterprise:
            #         result['warning'] = warn_enterprise
            #         result['expiration_date'] = ICP.get_param('database.expiration_date')
            #         result['expiration_reason'] = ICP.get_param('database.expiration_reason')
            #     return result
```
此处完毕。

4、将\odoo17-e-企业版\odoo_updated_modules17\web_enterprise\static\src\webclient\home_menu\enterprise_subscription_service.js第26行到31行注释失效并修改odoo有效期为6000年【this.expirationDate = DateTime.utc().plus({ years: 6000 });】，完整如下：

```js
    // if (session.expiration_date) {
    //     this.expirationDate = deserializeDateTime(session.expiration_date);
    // } else {
    //     // If no date found, assume 1 month and hope for the best
    //     this.expirationDate = DateTime.utc().plus({ days: 30 });
    // }
    this.expirationDate = DateTime.utc().plus({ years: 6000 });
```
此处完毕。

第118行到123行完整修改如下：
```js
// async checkStatus() {
//     await this.orm.call("publisher_warranty.contract", "update_notification", [[]]);

//     const expirationDateStr = await this.orm.call("ir.config_parameter", "get_param", [
//         "database.expiration_date",
//     ]);
//     this.lastRequestStatus = "update";
//     this.expirationDate = deserializeDateTime(expirationDateStr);
// }

async checkStatus() {
    await this.orm.call("publisher_warranty.contract", "update_notification", [[]]);
    const expirationDateStr = DateTime.now().plus({ years: 30 }).toLocaleString(DateTime.DATE_FULL);
    }
```
此处完毕

5、将\odoo17-e-企业版\odoo_updated_modules17\web_enterprise\static\src\webclient\settings_form_view\res_config_edition.xml全部改为：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<templates xml:space="preserve">

    <t t-inherit="web.res_config_edition" t-inherit-mode="extension">

        <!-- <xpath expr="//h3" position="replace">
            <h3 class="px-0">
                Odoo <t t-esc="serverVersion"/> (Enterprise Edition)
            </h3>
        </xpath> -->

        <xpath expr="//h3" position="replace">
            <h3 class="px-0">
                Odoo <t t-esc="serverVersion"/> (Ali Faleh Edition)
            </h3>
        </xpath>

        <xpath expr="//*[@id='license']" position="replace">
            <a id="license" target="_blank" href="https://www.odoo.com/documentation/17.0/legal/licenses.html" style="text-decoration: underline;">Odoo Enterprise Edition License V1.0</a>
        </xpath>

        <!-- <xpath expr="//h3" position="after">
            <t t-if="expirationDate">
                <h5>Database expiration: <t t-esc="expirationDate"/></h5>
            </t>
        </xpath> -->

        <xpath expr="//h3" position="after">
            <t t-if="expirationDate">
                <h5>This is an open version and has no expiration date.</h5>
            </t>
        </xpath>

    </t>

</templates>
```

