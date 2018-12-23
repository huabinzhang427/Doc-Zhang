需求：试卷功能，每个 Item 都有好几个选项，每个选项都是一个 Checkbox 控件，当我勾选完当前 Item 的选项后，控件状态会发生变化。到下一题继续勾选。但几次操作后返回查看，发现之前复选框的选项状态丢失。

如何保存复选框的状态？

## 操作

例子如下：

在 EduItemBean 对象中添加一个布尔变量 status，并保持项目的选择状态。

```java
public class EduItemBean {
    String code;
    String item;
    Boolean status = false; // 状态

    public Boolean getStatus() {
        return status;
    }

    public void setStatus(Boolean status) {
        this.status = status;
    }

    public String getCode() {
        return code;
    }

    public void setCode(String code) {
        this.code = code;
    }

    public String getItem() {
        return item;
    }

    public void setItem(String item) {
        this.item = item;
    }

    @Override
    public String toString() {
        return "EduItemBean{" +
                "code='" + code + '\'' +
                ", item='" + item + '\'' +
                ", status=" + status +
                '}';
    }
}
```
在 Adapter 中，设置和修改该状态。

```java
// ....
//in some cases, it will prevent unwanted situations
checkBox.setOnCheckedChangeListener(null);
//if true, your checkbox will be selected, else unselected
checkBox.setChecked(tests.get(i).getStatus());
if (checkBox.isChecked()) {
    checkBox.setTextColor(mContext.getResources().getColor(R.color.tab_text_s));
} else {
    checkBox.setTextColor(mContext.getResources().getColor(R.color.black2));
}
checkBox.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
    @Override
    public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
        tests.get(finalI).setStatus(isChecked);
        if (isChecked) {
            buttonView.setTextColor(mContext.getResources().getColor(R.color.tab_text_s));
        } else {
            buttonView.setTextColor(mContext.getResources().getColor(R.color.black2));
        }
    }
});            
```