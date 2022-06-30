# Architecture Design

Architecture is relatively simple. We are using [LitJS](https://lit.dev/) with [Tailwind](https://tailwindcss.com/) to build our widget and vanilla javascript to build the gateway script. The scripts and asset files are hosted on AWS S3 and served through Cloudfront.

There was an extensive research and decision making process prior to this and can be accessed at [Technical Spec For All Products Widget](https://confluence.ship.gov.sg/display/DEV/Technical+Spec+For+All+Products+Widget).

<iframe frameborder="0" style="width:100%;height:663px; border: 1px solid #ddd;" src="https://viewer.diagrams.net/?tags=%7B%7D&highlight=0000ff&edit=_blank&layers=1&nav=1#G1MhkaUVAzTQRx7KSSTQY06Vm_E7PejIzz"></iframe>