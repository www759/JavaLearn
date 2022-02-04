# ATM系统

```java
package BankDemo;

import java.util.ArrayList;
import java.util.Random;
import java.util.Scanner;


public class ATMSystem {
    public static void main(String[] args){
        ArrayList<Account> accounts = new ArrayList<>();

        showMain(accounts);
    }


    /**
     * 展示主界面
     * @param accounts 账户的集合对象
     */
    public static void showMain(ArrayList<Account> accounts){
        Scanner sc = new Scanner(System.in);

        while (true) {
            System.out.println("请你输入您想做的操作:");
            System.out.println("1. 登录");
            System.out.println("2. 注册");
            int command = sc.nextInt();
            switch (command) {
                case 1:
                    //登录
                    login(accounts);
                    break;
                case 2:
                    //注册
                    register(accounts);
                    break;
                default:
                    System.out.println("未找到命令, 请重新输入!");
            }
        }
    }

    /**
     * 开户功能
     * 1. 键盘录入姓名, 密码, 确认密码
     * 2. 生成账户卡号, 卡号必须由系统自动生成8位数字(唯一)
     * 3. 创建Account账户类对象用于封装账户信息
     * 4. 把对象存入集合accounts中
     * @param accounts 账户的集合对象
     */
    public static void register(ArrayList<Account> accounts){
        System.out.println("===========用户开户功能============");
        Scanner sc = new Scanner(System.in);

        //1. 输入姓名, 密码, 确认密码
        System.out.println("请输入您的姓名:");
        String userName = sc.next();
        String passward = "";
        while(true) {
            System.out.println("请输入您的密码:");
            passward = sc.next();
            System.out.println("请确认您的密码:");
            String passward1 = sc.next();
            if (!passward.equals(passward1)) {
                System.out.println("两次密码不同, 请重新输入!");
            }else break;
        }

        //2. 生成卡号
        String cardId = creatCardId(accounts);

        //3. 创建账户并添加至集合
        Account account = new Account(cardId, userName, passward, 0.0, 500.0);
        accounts.add(account);

        System.out.println("注册成功, 您的卡号为: " + cardId);
        //System.out.println(account.getCardId());
    }

    //创建卡号
    public static String creatCardId(ArrayList<Account> accounts){
        String cardId = "";

        while(true) {
            Random r = new Random();

            for (int i = 0; i < 8; ++i) {
                cardId += r.nextInt(10);
            }

            if(getAccountByCardId(cardId, accounts) == null){
                return cardId;
            }
        }

    }

    //通过卡号查新账户
    public static Account getAccountByCardId(String cardId, ArrayList<Account> accounts){
        for (int i = 0; i < accounts.size(); ++i) {
            Account account = accounts.get(i);
            if (account.getCardId().equals(cardId)) {
                return account;
            }
        }
        return null;
    }

    /**
     * 登录功能
     * 1. 录入账户, 密码, 并判断账户是否存在, 且密码是否正确
     * 2. 进入操作界面
     * @param accounts 用户账户对象集合
     */
    public static void login(ArrayList<Account> accounts){
        if(accounts.size() == 0){
            System.out.println("当前系统无任何账户, 请您先注册");
            return;
        }

        System.out.println("===========登录界面============");
        Scanner sc = new Scanner(System.in);

        //输入并验证账户
        Account account = null;
        while(true) {
            System.out.println("请您输入您的卡号:");
            String cardId = sc.next();
            account = getAccountByCardId(cardId, accounts);

            if (account == null) {
                System.out.println("您输入的卡号不存在, 请重新输入!");
            }else break;
        }

        //输入并验证密码
        while(true) {
            System.out.println("请输入您的密码:");
            String passward = sc.next();
            if (!account.getPassward().equals(passward)) {
                System.out.println("您输入的密码有误, 请重新输入!");
            } else break;
        }

        System.out.println("登录成功, 您的卡号是: " + account.getCardId());

        //进入操作页面
        showUserCommand(account, accounts);
    }

    /**
     * 用户操作
     * @param account
     */
    public static void showUserCommand(Account account, ArrayList<Account> accounts ){
        Scanner sc = new Scanner(System.in);
        while(true){
            System.out.println("============用户操作页面=============");
            System.out.println("1. 查询账户");
            System.out.println("2. 存款");
            System.out.println("3. 取款");
            System.out.println("4. 转账");
            System.out.println("5. 修改密码");
            System.out.println("6. 退出");
            System.out.println("7. 注销");


            int command = sc.nextInt();
            switch (command) {
                case 1:
                    //展示用户信息
                    showAccount(account);
                    break;
                case 2:
                    //存款
                    depositMoney(account);
                    break;
                case 3:
                    //取款
                    drawMoney(account);
                    break;
                case 4:
                    //转账
                    transferMoney(account, accounts);
                    break;
                case 5:
                    //修改密码
                    updatePassward(account);
                    return;
                case 6:
                    //退出
                    return;
                case 7:
                    //注销
                    //从当前集合抹去当前账户
                    accounts.remove(account);
                    System.out.println("销户成功！");
                    return;
                default:
                    System.out.println("您输入的命令有误");
            }
        }
    }

    public static void showAccount(Account account){
        System.out.println("============当前账户详情=============");
        System.out.println("卡号" + account.getCardId());
        System.out.println("姓名" + account.getUserName());
        System.out.println("余额" + account.getMoney());
    }

    public static void depositMoney(Account account){
        System.out.println("==========存款操作==========");
        System.out.println("请您输入存款的金额");
        Scanner sc = new Scanner(System.in);
        double money = sc.nextDouble();

        account.setMoney(account.getMoney() + money);
        System.out.println("存款完成");
        showAccount(account);
    }

    /**
     * 取款
     * @param account
     */
    public static void drawMoney(Account account){
        Scanner sc = new Scanner(System.in);

        System.out.println("==========取款操作==========");
        if(account.getMoney() >= 100){
            while (true) {
                System.out.println("请输入您要取的金额");
                double money = sc.nextDouble();

                if(money > account.getQuotaMoney()){//判断是否超过当此限额
                    System.out.println("您要取的金额大于当此限额, 请减少取款金额");
                }else if(money > account.getMoney()){//判断余额是否充足
                    System.out.println("余额不足");
                }else{//可取
                    account.setMoney(account.getMoney() - money);
                    System.out.println("取款成功!");
                    showAccount(account);
                    return;
                }
            }
        }else{
            System.out.println("您的余额小于100元, 无法取款");
        }
    }

    /**
     * 转账
     * 1. 判断系统是否有两个及以上个账户
     * 2. 判断自己账户是否有钱
     * 3. 输入对方卡号， 判断账户是否存在
     * 4. 对方账户存在还要认证对方的姓氏
     * 5. 满足要求则把自己的钱转至对方账户
     */
    public static void transferMoney(Account account, ArrayList<Account> accounts){
        System.out.println("==========转账操作==========");
        //判断是否有两个及以上账户
        if(accounts.size() < 2){
            System.out.println("系统中不存在其他账户， 无法转账");
            return;
        }
        Scanner sc = new Scanner(System.in);

        //判断账户金额
        System.out.println("请输入需要转入的金额");
        double money = sc.nextDouble();
        if(account.getMoney() < money){
            System.out.println("您的余额不足，无法转账！");
            return;
        }

        //判断账户是否存在及验证姓氏
        System.out.println("请输入需要转入的账户：");
        String toId = sc.next();
        Account toUser = getAccountByCardId(toId, accounts);
        if(toUser == null || toUser == account){
            System.out.println("账户不存在， 无法转账！");
            return;
        }else{
            System.out.print("您要转入的账户姓名为：");
            String name = toUser.getUserName();
            name = "*" + name.substring(1);
            System.out.println(name);

            while(true) {
                System.out.println("请确认对方姓氏：");
                char s = sc.next().charAt(0);
                if (s != toUser.getUserName().charAt(0)) {
                    System.out.println("输入错误， 请重新输入");
                }else break;
            }
        }

        //转入对方账户
        account.setMoney(account.getMoney() - money);
        toUser.setMoney(toUser.getMoney() + money);
        System.out.println("转账成功");
        showAccount(account);
    }


    /**
     * 修改密码
     */
    public static void updatePassward(Account account){
        System.out.println("==========修改密码=========");
        Scanner sc = new Scanner(System.in);

        System.out.println("请输入原密码：");
        String oldPassward = sc.next();
        if(!oldPassward.equals(account.getPassward())){
            System.out.println("原密码输入错误，无法修改！");
            return;
        }

        String newPassward = "";
        while(true) {
            System.out.println("请你输入需要修改的新密码：");
            newPassward = sc.next();

            System.out.println("请确认密码");
            String newPassward1 = sc.next();
            if (!newPassward.equals(newPassward1)) {
                System.out.println("两次密码不一致， 请重新输入");
            }else break;
        }
        account.setPassward(newPassward);
        System.out.println("修改密码成功！请你重新登录");
    }


}
```

```java
package BankDemo;

public class Account {
    private String cardId;  //卡号
    private String userName;    //客户名称
    private String passward;    //密码
    private double money;   //余额
    private double quotaMoney;  //当此取现限额

    public Account() {
    }

    public Account(String cardId, String userName, String passward, double money, double quotaMoney) {
        this.cardId = cardId;
        this.userName = userName;
        this.passward = passward;
        this.money = money;
        this.quotaMoney = quotaMoney;
    }

    public String getCardId(){
        return cardId;
    }

    public void setCardId(String cardId){
        this.cardId = cardId;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getPassward() {
        return passward;
    }

    public void setPassward(String passward) {
        this.passward = passward;
    }

    public double getMoney() {
        return money;
    }

    public void setMoney(double money) {
        this.money = money;
    }

    public double getQuotaMoney() {
        return quotaMoney;
    }

    public void setQuotaMoney(double quotaMoney) {
        this.quotaMoney = quotaMoney;
    }
}
```