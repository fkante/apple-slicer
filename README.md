# Apple Slicer

## What?
This script **parses iTunes Connect financial reports** and **splits sales by Apple subsidiaries** which are legally accountable for them.
It may be used to help generating *Reverse Charge* invoices for accounting and in order to correctly issue *Recapitulative Statements* mandatory in the EU.

## Why?

### In theory, selling apps is easy
You happily generate revenue with your apps on the App Store and Apple dutifully transfers the proceeds to you.

### In reality, though, ...
...you have to remit taxes for your App Store sales of course. For that reason, tax authorities require some sort of receipt for your income from App Store sales.
But will they accept the iTunes Connect Financial Reports? Or your bank statements?

Most probably they won't. This is why you need to generate receipts by invoicing Apple using the *Reverse Charge* procedure. Well, in bureaucratic Germany, at least.

### But wait! In Europe, there is more to do
For purchases that have been made in any member state of the European Union, Apple – in the role of your sales commissionaire – is accountable for remitting associated taxes.
For that reason, your local tax authorities require you to file a periodical *Recapitulative Statement* of those sales in order for them to be able to counter-check Apple's tax returns.

This is true if your business is based in the EU. In Germany, for example, the required tax document is called *Zusammenfassende Meldung*.

### Now for the problem
While due to Apple's internal cash pooling the wire transfer of your App Store proceeds is issued solely by Apple's European subsidiary in Luxembourg, namely iTunes S.á.r.l., the amount transmitted doesn't necessarily consist of European sales only.

In fact, because iTunes S.á.r.l. also collects revenue of other countries (at the time being f. ex. also Chinese and generally "rest of world" sales), it would therefore be wrong to declare the whole sum paid by iTunes S.á.r.l. in your *Recapitulative Statement*.

Instead, in order to correctly declare business done between you and Apple, **the sum paid by iTunes S.á.r.l. must be split into revenue made in member states of the European Union and into revenue made in Non-EU countries**.

### This is where this script comes handy
It breaks up App Store sales by country and assigns them to the specific Apple subsidiary which is legally accountable for them - for example,  *Apple Canada, Inc.* for Canadian or *iTunes K.K.* for Japanese sales.
The information which country is managed by which Apple subsidiary is taken from Schedule 2, Exhibit A of Apple's *Paid Applications* contract you signed when starting your App Store business.

You can use the output of the script to help genereating *Reverse Charge* invoices for accounting and also in order to correctly issue your *Recapitulative Statements*.

## How?

From iTunes Connect, download your financial reports of the desired billing month into an appropriately named directory.
For example, if you want your sales for January, 2015 to be split, download the financial reports for 2015/01 and move the resulting `*0115.txt` into a directory named `0115`.

Additionally, in order to display revenue in your local currency, the script needs the currency exchange rates and tax withholding amounts that were applicable at the time of the payment.
They aren't included in the financial report files, but luckily iTunes Connect has them available for you. In iTunes Connect, go to *Payments & Financial Reports* > *Payments* and just copy-paste the listing under *Earned / Paid on* corresponding to the financial report into a file named `currency_data.txt`. It should look similar to this:

```text
CHF      0.00    33.15   33.15   0.00    0.00    0.00    33.15   0.80030         26.53  EUR
EUR      0.00    206.89  206.89  0.00    0.00    0.00    206.89  1.00000         206.89 EUR
JPY      0.00    179     179     -37     0.00    0.00    142     0.00817         1.16   EUR
```

You are then ready to execute the script with the reports directory as parameter:

```sh
./slicer.py ~/Downloads/iTunesFinancialReports/0115
```
### Example output

The script generates tab-delimited output, more or less ready to be pasted into your favourite billing and/or tax return application.

```text
Sales date: 31.08.2014 - 27.09.2014 

iTunes S.á.r.l.
31-33 rue Sainte Zithe
2763 Luxembourg
Luxembourg
VAT ID: LU20165772

Sales in Finland (FI)
	Quantity Product	Amount		Exchange Rate	Amount in EUR
	1	 Example App 5	EUR 12,17	1.00000		12,17 €

Sales in France (FR)
	Quantity Product	Amount		Exchange Rate	Amount in EUR
	1	 Example App 5	EUR 12,17	1.00000		12,17 €

Sales in Switzerland (CH)
	Quantity Product	Amount		Exchange Rate	Amount in EUR
	2	 Example App 4	CHF 1,30	0.80030		1,04 €
	6	 Example App 3	CHF 7,80	0.80030		6,24 €
	5	 Example App 2	CHF 3,25	0.80030		2,60 €
	16	 Example App 1	CHF 20,80	0.80030		16,65 €

Sales in Germany (DE)
	Quantity Product	Amount		Exchange Rate	Amount in EUR
	2	 Example App 6	EUR 24,34	1.00000		24,34 €
	15	 Example App 5	EUR 158,21	1.00000		158,21 €

EU Total:	233,42 €


iTunes K.K.
〒 163-1480
3-20-2 Nishi-Shinjuku, Shinjuku-ku, Tokyo
Japan

Sales in Japan (JP)
	Quantity Product	Amount		Exchange Rate	Amount in EUR
	1	 Example App 4	JPY 60,00	0.00817		0,39 €
	1	 Example App 3	JPY 119,00	0.00817		0,77 €

JP Total:	1,16 €
```

You can configure your local currency (€ in this example) within the script.

## Obligatory disclaimer

There is absolutely no warranty. I am (thankfully) not a tax advisor and therefore cannot guarantee for the correctness of the above or the Python script in any way.
You need to verify for yourself if the above mentioned procedures are compatible with or even needed by your country's legislation.
Also, it is your obligation alone to check if the numbers the script computes are reasonable.

## Pull Requests
Neither English nor Python are my native language - corrective PRs are very welcome!
