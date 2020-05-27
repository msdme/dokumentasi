# Dokumentasi API Zendesk yang digunakan di Sociomile untuk Forward Tiket Sociomile dan dibuat baru di Zendesk.

- Versi: 1.0
- Dibuat oleh: [Masdi](mailto:masdi@ivosights.com)
- Update Terakhir: 27 Mei 2020

Ini adalah dokumentasi untuk mengetahui API mana saja yang dipakai di Sociomile untuk fitur Forward ke Zendesk.

Dokumentasi ini digunakan untuk kebutuhan integrasi Bukalapak x Sociomile.

Segala dokumentasi yang lebih jelas harap gunakan dokumentasi resmi dari [Zendesk](https://developer.zendesk.com/rest_api/docs/support/introduction).

Untuk parameter _request_ yang tidak dikirimkan dari Sociomile, namun dibutuhkan untuk forward Bukalapak, dapat di diskusikan lebih lanjut.

---
- _Expect response_ untuk semua API adalah bentuk original dari Zendesk.
- _Expect Header_ untuk semua API adalah parameter original dari Zendesk, namun tidak menutup kemungkinan ada kustomisasi khusus untuk authentication header.
---

# Bentuk Form Forward di Sociomile

[Klik disini untuk melihat Form Upload Image dari Sociomile](https://pasteboard.co/Ja82oPp.png)

# Daftar API yang digunakan di Sociomile
## Create Ticket:
API utama yang digunakan adalah API Create Ticket ini. Namun untuk menggunakan API ini, diperlukan penggunaan API yang lainnya dari Zendesk.

[ ke dokumentasi zendesk ](https://developer.zendesk.com/rest_api/docs/support/tickets#create-ticket)


> Method: 
```
POST
```
> Url yang digunakan di Zendesk: 
```
/api/v2/tickets.json
```

> Headers: 



```
Content-Type: application/json
Auth: [ "*username*/token","*token*" ]
```
Contoh Headers:
```
Content-Type: application/json
Auth: [ "masdi@ivosights.com/token","msKjbKtdPGa7XWfh" ]

```


### Parameter yang digunakan di Sociomile:
```
{
    "subject" : *subject dari sociomile [string]*,
    "priority" : "normal",
    "type" : "ticket",
    "external_id": *ticket number dari sociomile [string]*,
    "tags":*tags dari zendesk (scroll ke bawah untuk lihat dokumentasi)[array-objects]*,
    "via" :{
        "channel" : "email",
        "source" :{
            "from" :{
                "address" : *email dari sociomile[string]*,
                "name" : *real name dari sociomile[string]*
            }
        }
    },
    "comment" :{
        "type" : "comment",
        "value" : *stream dari sociomile[string]*,
        "requester_id" : *requester id dari zendesk (scroll ke bawah untuk lihat dokumentasi)[integer]*,
        "submitter_id" : *requester id dari zendesk (scroll ke bawah untuk lihat dokumentasi)[integer]*,
        "created_at" : *time dari sociomile[string|date|2020-12-31 23:59:59]*,
        "updated_at" : *time dari sociomile[string|date|2020-12-31 23:59:59]*,
    }
    ,
    "group_id" : *ID Grup User dari Zendesk (scroll ke bawah untuk lihat dokumentasi)[nullable|integer]*,
    "follower" : *ID Agent yang ditag sebagai follower tiket dari Zendesk (scroll ke bawah untuk lihat dokumentasi)[array-integers|nullable]*,
    "assignee_id" : *ID Agent yang diassign untuk menangani tiket dari Zendesk (scroll ke bawah untuk lihat dokumentasi)[integer|nullable]*,
    
    "sharing_agreement_ids":["360000057392"], // <-- id dari API Zendesk (scroll ke bawah untuk lihat dokumentasi)
     
    "ticket_form_id":*id form dari Zendesk (scroll ke bawah untuk lihat dokumentasi)[integer]*, 
    "custom_fields":[
        {
            "id" : "360017427311", // <- id custom fields dari zendesk (scroll ke bawah untuk lihat dokumentasi)
            "value" : "incident"// <- contoh value yang diinput di form[string|nullable]
        }
    ] <-- id dan value dari custom fields yang diisi di form, ID didapat dari API Zendesk (scroll ke bawah untuk lihat dokumentasi)
}
```


Keterangan:

> Untuk parameter yang menggunakan double quote (seperti parameter "priority":"normal"), adalah nilai statis.
---

## Tags: 

Pada [dokumentasi support zendesk](https://support.zendesk.com/hc/en-us/articles/203662096-About-tags) tags tidak hanya dapat digunakan pada tiket, namun tags yang digunakan disini hanya tags untuk membuat tiket.

[lihat dokumentasi api zendesk](https://developer.zendesk.com/rest_api/docs/support/tags#autocomplete-tags)

- Digunakan untuk mengisi field: `tags`

> Method
```
GET
```
> Url yang digunakan di Sociomile

```
/api/v2/autocomplete/tags.json
```


> Contoh bentuk request yang digunakan di Sociomile
```
[subdomain-dari-zendesk.zendesk.com]/api/v2/autocomplete/tags.json?name={string}
```
---

## Asignee:
Untuk Assignee ini dibutuhkan 3 API yang di gunakan dari Zendesk, kemudian di bentuk ulang di backend Sociomile untuk digunakan di form forward Sociomile.

### API 1. Get Group Membership
[lihat dokumentasi api zendesk](https://developer.zendesk.com/rest_api/docs/support/group_memberships#list-memberships)
- Digunakan untuk mengisi field: ```assignee_id```

> Method
```
GET
```
> Url yang digunakan di Sociomile
```
/api/v2/group_memberships.json
```
> Contoh bentuk request yang digunakan Sociomile
```
[zendesk-subdomain]/api/v2/group_memberships.json
```
 


### API 2. Get Assignable Group
[lihat dokumentasi api zendesk](https://developer.zendesk.com/rest_api/docs/support/groups#show-assignable-groups
)

> Method 
```
GET
```
> Url yang digunakan di Sociomile
```
/api/v2/groups/assignable.json
```

> Contoh bentuk request yang digunakan Sociomile
```
[subdomain-zendesk]/api/v2/groups/assignable.json
```

### API 3. Get Users

- lihat dokumentasi dibawah untuk `Followers`

---

## Followers: 


[lihat dokumentasi api zendesk](https://developer.zendesk.com/rest_api/docs/support/users#list-users)
- Digunakan untuk mengisi field: ```follower```

> Method
```
GET
```
> Url yang digunakan di Sociomile
```
/api/v2/users.json
```
> Contoh bentuk request yang digunakan Sociomile
```
[zendesk-subdomain]/api/v2/users.json?role[]=admin&role[]=agent
```

---

## Create or Update User

User yang dibuat adalah user customer berdasarkan email yang diinput.
- Digunakan untuk mengisi field: ```requester_id, submitter_id```
[lihat dokumentasi api Zendesk](https://developer.zendesk.com/rest_api/docs/support/users#create-or-update-user)
> Method 
```
POST
```
> Url yang digunakan di Sociomile
```
/api/v2/users/create_or_update.json
```
> Contoh bentuk request yang digunakan di Sociomile
```
{
    "user":{
        "name":*nama pelanggan/customer dari Sociomile*,
        "email":*email yang diinput di Sociomile*,
    }
}

```

## Forms:
Forms ini didapat dari 2 API dari Zendesk, kemudian di bentuk ulang di backend Sociomile untuk digunakan di form forward Sociomile.


### API 1. Get Ticket Forms
[lihat dokumentasi API Zendesk](https://developer.zendesk.com/rest_api/docs/support/ticket_forms#list-ticket-forms)

- Digunakan untuk mengisi field: ```ticket_form_id```
> Method
```
GET
```
> Url yang digunakan di Sociomile
```
/api/v2/ticket_forms.json
```
> Contoh request yang digunakan di Sociomile
```
[zendesk-subdomain]/api/v2/ticket_forms.json
```

### API 2. Get Ticket Fields

[lihat dokumentasi API Zendesk](https://developer.zendesk.com/rest_api/docs/support/ticket_fields#list-ticket-fields)
- Digunakan untuk mengisi field: ```custom_fields, custom_fields.*.id```

> Method
```
GET
```
> Url yang digunakan di Sociomile
```
/api/v2/ticket_fields.json
```
> Contoh request yang digunakan di Sociomile
```
[zendesk-subdomain]/api/v2/ticket_fields.json
```

---

## Sharing Agreements:

- [lihat dokumentasi API Zendesk](https://developer.zendesk.com/rest_api/docs/support/sharing_agreements#list-sharing-agreements)
- [lihat dokumentasi Support Zendesk](https://support.zendesk.com/hc/en-us/articles/203661466-Sharing-tickets-with-other-Zendesk-Support-accounts)

- Digunakan untuk megisi field: ```sharing_agreement_ids```

> Method
```
GET
```
> Url yang digunakan di Sociomile
```
/api/v2/sharing_agreements.json
```
> Contoh request yang digunakan di Sociomile
```
[zendesk-subdomain]/api/v2/sharing_agreements.json
```
