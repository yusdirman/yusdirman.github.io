---
layout: post
title:  "Render pdf from HTML using widked_pdf"
date:   2017-10-29 14:30:00 +0800
categories: Ruby on Rails, pdf, html to pdf
---

# Rendering pdf from HTML using wicked_pdf

Assalamualaikum.

I'm using wicked_pdf to generate pdf file direct from HAML templates within my Ruby on Rails application. In this guide, I would like to share how I render pdf file inline to print charge sheet for customer.

## Install gem

```ruby
# generate PDF from html to print
gem 'wicked_pdf'
# required by wicked_pdf
gem 'wkhtmltopdf-binary'
```

## Create templates

I've created haml templates at
`views/layouts/pdf/charge_sheet.pdf.haml`
`views/layouts/pdf/header.pdf.haml`
`views/layouts/pdf/footer.pdf.haml`

```ruby
# views/layouts/pdf/charge_sheet.pdf.haml

!!!
%html
  %head
    %title PDF
    = wicked_pdf_stylesheet_link_tag "pdf"
  - bg_color = Rails.env.development? ? "" : "#eee2ef";
  %body{style: "background-color: #{bg_color}"}
    .row
      .col
        = yield

# views/layouts/pdf/header.pdf.haml

.container-fluid
  .row
    .col
      - if Rails.env.development?
        %p.text-left
          %h5.text-danger Sample for development
    .col
      %p.text-right
        = ItemRequest::CHARGE_SHEET_ISO_NUMBER
  %br
  .row
    .col-5
      = wicked_pdf_image_tag 'company_logo.jpg', class: "img-fluid rounded float-left"
    .col-7
      %strong
        Charge Sheet for Medical Implant and Devices
        %br
        Yavin4 Community Hospital

# views/layouts/pdf/footer.pdf.haml
!!!
%html
  %head
    :javascript
      function number_pages() {
        var vars={};
        var x=document.location.search.substring(1).split('&');
        for(var i in x) {var z=x[i].split('=',2);vars[z[0]] = decodeURIComponent(z[1]);}
        var x=['frompage','topage','page','webpage','section','subsection','subsubsection'];
        for(var i in x) {
          var y = document.getElementsByClassName(x[i]);
          for(var j=0; j<y.length; ++j) y[j].textContent = vars[x[i]];
        }
      }
  %body{:onload => "number_pages()"}
    .container.footer
      .row
        .col-10
          %small
            %strong
              Notes: Patient needs to make payments at our front cashier
            %br
            Any enquiries please call Tel: 98123-29-733-3060-YAVIN,
            email: master_yusdirman@yavin4.my |
        .col-2
          Page
          %span.page
          of
          %span.topage

```

1. noticed that the sylesheet tag is using `wicked_pdf_stylesheet_link_tag`
2. i've downloaded twitter/bootstrap v4.0 and imported it into `pdf.css.scss` inside `assets/styleshetts/`
3. I've also set the backgroud-color to bluish in development environment.
4. There is also another similar tag,  `wicked_pdf_image_tag` for importing image to our pdf document.
5. Also noticed that in footer.pdf.haml, there are javascripts coding to display page number.

this is the content of pdf.css.scss
```scss
@import "bootstrap4.min.css";

body{
  font-size: 10pt;
  padding: 1px;
}

table{
  font-size: 9pt;
}

thead { display: table-header-group }
tfoot { display: table-row-group }
tr { page-break-inside: avoid }

.footer{
  page-break-before: auto;
}
```
> Table thead and page-break
>   when a pdf create a new page, and inside the rendered content is a table, the css setting for thead, tfoot and tr is necessary to ensure the table header doesn't overlapped on page 2.


## Generate PDF
```ruby
def generate_pdf_string
    return render_to_string pdf: "charge-sheet",
    template: 'item_requests/charge_sheet',
    layout: "layouts/pdf/charge_sheet.pdf.haml",
    encoding: 'UTF-8',
    page_size: 'A4',
    orientation: 'Portrait',
    locals: {item_request: @item_request, patient: @patient, request_items: @request_items},
    margin: { top: 38, bottom: 18 , left: 5 , right: 5},
    header: {
      html:{
        template: 'layouts/pdf/header.pdf.haml'
      }
    },
    footer: {
      html:{
        template: 'layouts/pdf/footer.pdf.haml'
      },
      locals: {page: '[page]', topage: '[topage]'}
    }
end
```

> In order to be able to save the pdf somewhere, or perhaps to email the pdf afterwards, I've come to solution to generate the dpf as tring and then sent to browser as application/pdf


# the Controller

```ruby
# GET /item_request/1/charge_sheet
  def charge_sheet
    # @current_user is from session
    # @item_request is from action filter

    pdf = generate_pdf_string
    cs = ChargeSheet.new(pdf_string: pdf, filetype: 'application/pdf', user_id: @current_user.id, item_request_id: @item_request.id)
    if cs.save
      send_data(pdf, type: 'application/pdf', filename:'charge_sheet.pdf', disposition: 'inline')
    else
       @item_request, alert: "Error Saving Charge Sheet: #{cs.errors.messages} "
    end
  end
```

so, I set up a button to this controller and a new tab opened up with the rendered pdf file.

I hope that helps.


Thanks..

yusdirman
