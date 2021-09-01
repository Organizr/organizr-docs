# Hide custom text from specific groups

## Summary

To display text only for specific user groups.

In this example, we are using groupID `999` which is the Guest Group.

```text
<script>
    if(activeInfo.user.groupID !== 999){
        var cssSettings = `
            .hidden-for-non-guest {
                display: none;
            }
        `;
        $('#guest-css').html(cssSettings);
    }
</script>
<style id="guest-css"></style>
<div class="hidden-for-non-guest">
    <h1>You're a guest.  Please log in!</h1>
</div>
This is text everyone sees.
```

### Steps

Open the Tab Editor and go to Homepage Items then finally select Custom HTML

![](../.gitbook/assets/image%20%285%29.png)

Next you will Click which Custom HTML segment you want to use and `Enable`it then change the `Minimum Authentication` to the group you want to allow to see it.  Next you will copy the code from above and paste it into the box at the bottom.  Finally hit the 💾 button.

![](../.gitbook/assets/image%20%282%29.png)

### Outcome

#### A User who is logged in

![](../.gitbook/assets/image%20%287%29.png)

#### A User who is not logged in

![](../.gitbook/assets/image%20%281%29.png)

