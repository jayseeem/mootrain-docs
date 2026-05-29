# Order Cancel Checker


[🎯 Cancel Check]{ .md-button .md-button--primary }

1. Drag the button above to your bookmarks bar
1. Go to Target.com and sign in
1. Click the bookmark & enter your order number

    While on Target.com, click the "🎯 Cancel Check" bookmark you saved. A popup will ask for your order number — type it in and hit OK.
1.

``` { .js .no-copy }
javascript: void (function () {
    if (!location.hostname.includes("target.com")) {
        alert("Please%20go%20to%20Target.com%20first,%20sign%20in,%20then%20click%20this%20bookmark%20again.");
        return;
    }
    var% 20n=prompt("Enter%20your%20Target%20order%20number:");
    if (!n || !n.trim()) return;
    n = n.trim();
    fetch("https://api.target.com/guest_order_aggregations/v1/" + encodeURIComponent(n) + "?key=f5e5f35b610d54dff3f3c9087c837f479f22686e%22, {
    credentials:% 22include % 22 }
).then(function (r) {
    if (!r.ok) throw% 20new % 20Error(% 22Order % 20not % 20found % 20(HTTP % 20 % 22 + r.status +% 22) % 22);
    return% 20r.json();
}
).then(function (d) {
    var% 20t = document.createElement(% 22textarea % 22);
    function% 20dec(s) {
        t.innerHTML = s ||% 22 % 22;
        return% 20t.value;
    }
    var% 20m =% 22ORDER % 20CANCEL % 20DETAILS\n ========================\n\n % 22;
    m +=% 22Fraud % 20Status:% 20 % 22 + (d.fraud_status ||% 22Unknown % 22) +% 22\n % 22;
m +=% 22Order % 20#:% 20 % 22 + (d.order_number || d.external_order_number ||% 22N / A % 22) +% 22\n % 22;
if (d.placed_date) m +=% 22Placed:% 20 % 22 + new% 20Date(d.placed_date).toLocaleString() +% 22\n % 22;
if (d.order_lines && d.order_lines.length) {
    d.order_lines.forEach(function (l, i) {
        var% 20s = (l.fulfillment_spec && l.fulfillment_spec.status) ? l.fulfillment_spec.status : {
        }
            ;
        var% 20it = l.item || {
        }
            ;
        m +=% 22\n------------------------\n % 22;
        m +=% 22Item % 20 % 22 + (i + 1) +% 22:% 20 % 22 + dec(it.description ||% 22Unknown % 22) +% 22\n % 22;
        m +=% 22Qty:% 20 % 22 + (l.original_quantity ||% 22 ?% 22) +% 22 % 20 |% 20Price:% 20$ % 22 + (it.unit_price ||% 22 ?% 22) +% 22\n % 22;
        var% 20st = (l.grouping && l.grouping.name) ? l.grouping.name.replace(% 22STAT_ % 22,% 22 % 22) :% 22 % 22;
        if (st) m +=% 22Status:% 20 % 22 + st +% 22\n % 22;
        if (s.cancel_reason_code_description) m +=% 22\n % 3E % 3E % 20Cancel % 20Reason:% 20 % 22 + s.cancel_reason_code_description +% 22\n % 22;
        if (s.cancel_reason_text) m +=% 22 % 3E % 3E % 20Details:% 20 % 22 + s.cancel_reason_text +% 22\n % 22;
        if (s.cancel_reason_code) m +=% 22 % 3E % 3E % 20Code:% 20 % 22 + s.cancel_reason_code +% 22\n % 22;
    }
    );
}
alert(m);
    }
    ).catch (function(e) {
    alert(% 22Error:% 20 % 22 + e.message +% 22\n\nMake % 20sure % 20you % 20are % 20signed % 20into % 20Target.com % 20and % 20the % 20order % 20number % 20is % 20correct.% 22);
}
    );
 }
) ()
```

## Cancellation Reasons

### CANCEL_FRAUD
Target's automated system flagged your order. This doesn't mean you did anything wrong — it often triggers for things like ordering multiple high-demand items, using a new address, or buying popular products with purchase limits.

### Item Demand

### Reseller

### Quantity Limit

[🎯 Cancel Check]: javascript:void(function(){if(!location.hostname.includes("target.com")){alert("Please%20go%20to%20Target.com%20first,%20sign%20in,%20then%20click%20this%20bookmark%20again.");return;}var%20n=prompt("Enter%20your%20Target%20order%20number:");if(!n||!n.trim())return;n=n.trim();fetch("https://api.target.com/guest_order_aggregations/v1/"+encodeURIComponent(n)+"?key=f5e5f35b610d54dff3f3c9087c837f479f22686e%22,{credentials:%22include%22}).then(function(r){if(!r.ok)throw%20new%20Error(%22Order%20not%20found%20(HTTP%20%22+r.status+%22)%22);return%20r.json();}).then(function(d){var%20t=document.createElement(%22textarea%22);function%20dec(s){t.innerHTML=s||%22%22;return%20t.value;}var%20m=%22ORDER%20CANCEL%20DETAILS\n========================\n\n%22;m+=%22Fraud%20Status:%20%22+(d.fraud_status||%22Unknown%22)+%22\n%22;m+=%22Order%20#:%20%22+(d.order_number||d.external_order_number||%22N/A%22)+%22\n%22;if(d.placed_date)m+=%22Placed:%20%22+new%20Date(d.placed_date).toLocaleString()+%22\n%22;if(d.order_lines&&d.order_lines.length){d.order_lines.forEach(function(l,i){var%20s=(l.fulfillment_spec&&l.fulfillment_spec.status)?l.fulfillment_spec.status:{};var%20it=l.item||{};m+=%22\n------------------------\n%22;m+=%22Item%20%22+(i+1)+%22:%20%22+dec(it.description||%22Unknown%22)+%22\n%22;m+=%22Qty:%20%22+(l.original_quantity||%22?%22)+%22%20|%20Price:%20$%22+(it.unit_price||%22?%22)+%22\n%22;var%20st=(l.grouping&&l.grouping.name)?l.grouping.name.replace(%22STAT_%22,%22%22):%22%22;if(st)m+=%22Status:%20%22+st+%22\n%22;if(s.cancel_reason_code_description)m+=%22\n%3E%3E%20Cancel%20Reason:%20%22+s.cancel_reason_code_description+%22\n%22;if(s.cancel_reason_text)m+=%22%3E%3E%20Details:%20%22+s.cancel_reason_text+%22\n%22;if(s.cancel_reason_code)m+=%22%3E%3E%20Code:%20%22+s.cancel_reason_code+%22\n%22;});}alert(m);}).catch(function(e){alert(%22Error:%20%22+e.message+%22\n\nMake%20sure%20you%20are%20signed%20into%20Target.com%20and%20the%20order%20number%20is%20correct.%22);});})()