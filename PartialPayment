$(document).ready(function () {
    // -------------------- Utility Functions --------------------
    function parseCurrency(value) {
        if (!value) return 0;
        value = value.toString().replace(/[^\d.-]/g, '');
        const num = parseFloat(value);
        return isNaN(num) ? 0 : num;
    }

    function lockField($field) {
        if ($field.is("select") || $field.is(":radio") || $field.attr("type") === "file") {
            $field.prop("disabled", true); // disable but keep value
        } else {
            $field.prop("readonly", true);
        }
        $field.addClass("locked");
        $field.trigger('change');
    }

    function hideField($field) {
        $field.closest('div').hide();
    }

    // -------------------- Remove Delete Buttons --------------------
    function removeDeleteButtons() {
        $(".cf-collection-delete").remove();
        $(".file-del").remove();
    }

    // Run on load
    removeDeleteButtons();

    // -------------------- Payment Config --------------------
    const buttonMap = {
        "calculateOutstandingBtn_Cheque": "Field74",
        "calculateOutstandingBtn_Card": "Field129",
        "calculateOutstandingBtn_Direct": "Field110",
        "calculateOutstandingBtn_Insurer": "Field122",
        "calculateOutstandingBtn_Sheppard": "Field126"
    };

    const extraFieldsMap = {
        "Cheque": 	["Field102", "Field71", "Field103", "Field75"],
        "Card": 	["Field102", "Field103", "Field75", "Field104", "Field105", "Field106", "Field103"],
        "Direct": 	["Field102", "Field103", "Field75", "Field111"],
        "Insurer": 	["Field102", "Field103", "Field75", "Field123"],
        "Sheppard": ["Field102", "Field103", "Field75", "Field127", "Field125"]
    };

    const maxPayments = 20;

    // -------------------- Check for Previous Payment --------------------
    function previousPaymentExists() {
        let foundPayment = false;
        $.each(buttonMap, function(btnClass, baseFieldId){
            for(let i=1; i<=maxPayments; i++){
                const val = parseCurrency($("[id='" + baseFieldId + "(" + i + ")']").val());
                if(val > 0){
                    foundPayment = true;
                    return false;
                }
            }
            if(foundPayment) return false;
        });
        return foundPayment;
    }

    // -------------------- Lock Existing Payments --------------------
    function lockExistingPayments() {
        $.each(buttonMap, function(btnClass, baseFieldId){
            const method = btnClass.replace("calculateOutstandingBtn_", "");
            for(let i=1; i<=maxPayments; i++){
                const $field = $("[id='" + baseFieldId + "(" + i + ")']");
                if($field.length && parseCurrency($field.val()) > 0){
                    lockField($field);

                    const extraFields = extraFieldsMap[method];
                    if(extraFields){
                        extraFields.forEach(f=>{
                            const $extra = $("[id^='" + f + "(" + i + ")']");
                            if($extra.length){
                                $extra.each(function(){
                                    const $el = $(this);
                                    if($el.is("select") || $el.is(":radio") || $el.attr("type") === "file"){
                                        $el.prop("disabled", true).addClass("locked");
                                    } else {
                                        lockField($el);
                                    }
                                });
                            }
                        });
                    }
                }
            }
        });

        const $outstanding = $("#Field100");
        if(parseCurrency($outstanding.val()) > 0) lockField($outstanding);
        lockField($("#Field58"));

        removeDeleteButtons();
    }
  	const $outstanding = $("#Field100");
	lockField($outstanding);
    // -------------------- Conditional Page Load --------------------
    const fullPartialValue = $("#Field100").val();
    const hasPrevPayment = previousPaymentExists();

    if(fullPartialValue && fullPartialValue.trim() !== "" && hasPrevPayment){
        console.log("🔒 Previous partial payment detected — locking all related fields.");
        lockExistingPayments();
    } else {
        console.log("🟢 No previous payment found — fields remain editable.");
    }

    // -------------------- Apply Latest Payment --------------------
    function applyPayment(baseFieldId, method) {
        let outstanding = parseCurrency($("#Field100").val());
        let latestIndex = -1;
        let latestValue = 0;

        for(let i=1; i<=maxPayments; i++){
            const $field = $("[id='" + baseFieldId + "(" + i + ")']");
            if($field.length && parseCurrency($field.val()) > 0 && !$field.hasClass("locked")){
                latestIndex = i;
                latestValue = parseCurrency($field.val());
            }
        }

        if(latestIndex === -1){
            console.warn(`No new payment found for ${baseFieldId}`);
            return;
        }

        if(latestValue > outstanding){
            alert(`⚠️ Payment of ${latestValue.toFixed(2)} exceeds the outstanding amount of ${outstanding.toFixed(2)}.`);
            return;
        }

        outstanding -= latestValue;
        $("#Field100").val(outstanding.toFixed(2)).trigger("change");

        const $latestField = $("[id='" + baseFieldId + "(" + latestIndex + ")']");
        lockField($latestField);

        const extraFields = extraFieldsMap[method];
        if(extraFields){
            extraFields.forEach(f=>{
                const $extra = $("[id^='" + f + "(" + latestIndex + ")']");
                if($extra.length){
                    $extra.each(function(){
                        const $el = $(this);
                        if($el.is("select") || $el.is(":radio") || $el.attr("type") === "file"){
                            $el.prop("disabled", true).addClass("locked");
                        } else {
                            lockField($el);
                        }
                    });
                }
            });
        }

        removeDeleteButtons();

        console.log(`Applied ${latestValue} from ${baseFieldId}(${latestIndex}). Remaining outstanding: ${outstanding}`);

        if(outstanding === 0){
            alert("✅ Outstanding payment is fully completed!");
        }
    }

    // -------------------- Button Handlers --------------------
    $.each(buttonMap, function(btnClass, baseFieldId){
        const method = btnClass.replace("calculateOutstandingBtn_", "");
        $(document).on("click", "." + btnClass, function(e){
            e.preventDefault();
            applyPayment(baseFieldId, method);
        });
    });

    // -------------------- Keep Locked When Adding New Payment --------------------
    $(document).on("click", ".cf-collection-append", function() {
        setTimeout(function() {
            lockExistingPayments();
            removeDeleteButtons();
        }, 200); // delay to allow new row to be added before re-locking
    });

    // -------------------- Comments Section --------------------
  
  	
    $('#q113 input').val($('#q114').text()).trigger('change');
    var initial = $(".user input").val();
    var initials = "👤 " + initial;

    $(".comment").on('click', function () {
        var message = $('.allcomments textarea').val();
        var dt = new Date();
        var hr = dt.getHours();
        var min = dt.getMinutes();
        var sec = dt.getSeconds();
        var ampm = 'AM';
        if(hr>12){ hr -= 12; ampm="PM"; }
        if(hr<10) hr="0"+hr;
        if(min<10) min="0"+min;
        if(sec<10) sec="0"+sec;
        var ds = (dt.getMonth()+1)+"/"+dt.getDate()+"/"+dt.getFullYear().toString().substr(-2)
                 +" - "+hr+":"+min+":"+sec+" "+ampm;

        $('.allcomments textarea').val($('.usercomment textarea').val()+" - "+initials+" - "+ ds+"\n\n"+message).change();
        $('.usercomment textarea').val("").change();
        $('.Submit').show();
    });

    $('.allcomments textarea').keydown(function(e){
        e.preventDefault();
        return false;
    });

    $('.usercomment').on('change keyup keydown paste cut','textarea',function(){
        $('.Submit').show();
        if($('.usercomment textarea').val()!="") $('.Submit').hide();
    });

    $('.allcomments').on('change keyup keydown paste cut','textarea',function(){
        $(this).height(0).height(this.scrollHeight);
    }).find('textarea').change();
});
