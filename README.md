**PhxPaxos����Ѷ��˾΢�ź�̨�Ŷ������з���һ�׻���PaxosЭ��Ķ��״̬������⡣���Կ⺯���ķ�ʽǶ�뵽�����ߵĴ��뵱�У�
ʹ��һЩ����״̬���������չ����������Ӷ����ǿһ���ԵĶั���Լ��Զ����ֵ����ԡ�**
**��������΢�ŷ������澭��һϵ�еĹ�����֤���������Ƕ������й������Ķ��ӻ����µĲ��ԣ�ʹ����һ���Եı�֤�ϸ�Ϊ��׳��**

���ߣ�Haochuan Cui (lynncui@tencent.com), Ming Chen (mingchen@tencent.com), Junchao Chen (junechen@tencent.com) �� Duokai Huang (mariohuang@tencent.com) 

# ����
  * ����Lamport��Paxos Made Simple���й��̻����������κ��㷨���֡�
  * ʹ�û�����Ϣ���ݻ��ƵĴ��첽���̼ܹ���
  * ÿ��д��ʹ��fsync�ϸ�֤��ȷ�ԡ�
  * һ��Propose��д�����ݣ���LatencyΪһ��RTT����̯����д�̴���Ϊ1�Ρ�
  * ʹ�õ�Ե���ʽЭ����п���ѧϰ��
  * ֧��Checkpoint�Լ���PaxosLog���Զ�����
  * ֧�ֿ������Checkpoint�Զ���ȡ��
  * һ��PhxPaxosʵ������ͬʱ���ض��״̬����
  * ��ʹ�þ���״̬��ģʽ����Checkpoint���Զ����ɡ�
  * ����Masterѡ�ٹ��ܡ�
  * �������ݵ�ʵʱ����checksumУ�顣
  * ���硢�洢����ء���־ģ�����������ɿ������Զ��塣
  
# ����
  * һ��PhxPaxosʵ����һʱ��ֻ���������ڵ�һ���̣�������̣߳���
  * ������û���ڽ���client-server��֧�֣������߱��뽫���Ĵ���Ƕ�뵽�Լ��ķ������������棬��ʵ��������ܡ�
  * PhxPaxosֻ����������64λ��Linuxƽ̨��
  
# ����
### ���л���

    CPU��24 x Intel(R) Xeon(R) CPU E5-2420 0 @ 1.90GHz
    �ڴ棺32 GB
    Ӳ�̣�ssd raid5
    ������ǧ������
    ��Ⱥ���������� 3��
    ��Ⱥ�����PINGֵ�� 0.05ms
    ����д�벢����100���߳�
    
### ���ܲ��Խ��(qps)
###### д��С����(100B)
    1��ʵ���� 1171
    20��ʵ���� 11931
    50��ʵ���� 13424
    100��ʵ���� 13962
###### д�������(100KB)
    1��ʵ���� 280
    20��ʵ���� 984
    50��ʵ���� 1054
    100��ʵ���� 1067

# ����Ŀ¼����
**include**Ŀ¼������ʹ��PhxPaxos����Ҫ�õ�������ͷ�ļ�������Ҫ�����Щͷ�ļ��������ຯ���ĺ��壬������ȷ��ʹ��PhxPaxos��
>ע�����Ƕ��⹫��API���������Ŀ¼��ͷ�ļ���������������÷Ǵ�Ŀ¼��ͷ�ļ���һЩ�ڲ�API��
��ЩAPI�����п��ܻ���ʱ�Ľ��е����������м��ݡ�

**src**Ŀ¼���ڷ���PhxPaxos����Ҫ�õ���һЩ�������⣻һ��ջ��PhxPaxosԴ�����ʱ������һ����Ŀ¼��
��η��õ��������ں�����뷽����½ڻ���ϸ���ܡ�ʹ��PhxPaxos��Ҫ�����������������⣬�ֱ���protobuf��leveldb��

**plugin**Ŀ¼�ṩ��־���ģ�顣��־�ǵ��Գ������Ҫ��ʽ������ͬ��֮֯�����־����ͨ���кܴ������
���PhxPaxos��srcĿ¼��δ�ṩ�������־���ܣ������ṩ����־�Ĳ�����ƣ�ʹ����־���ܿ��Ա����ơ�Ϊ�˷����ҿ��ٳ���PhxPaxos�⣬���ǻ���Ŀǰ�Ƚ����е�glog��ʵ����һ����־����������պ�Ҳʹ��glog����ôpluginĿ¼�Ĵ������Ϊ�����һЩ����ʱ�䡣

**sample**Ŀ¼�ṩ�˻���PhxPaxosʵ�ֵ�������������������ֱ��Ӧ��ʹ��PhxPaxos����ǳ����Ӽ򵥵����ŵ����׵Ĳ�ͬ�׶Ρ�
 * PhxElection��һ����Ϊ�򵥵ĳ�����������PhxPaxos������Masterѡ�ٹ��ܣ�ʵ����һ��ѡ��Master�ĳ���
 * PhxEchoչʾ����α�дһ��״̬��������Phxpaxos��ϡ�
 * PhxKV����һ����Ϊ������ϵͳ����ʵ����һ��KV��״̬��������PhxPaxosʵ���˷ֲ�ʽKV�洢����չʾ�����ʵ��Checkpoint��ɾ��paxos log��
 ������ͬʱչʾ����ô����Щ�������ϵ�һ��RPC��ܣ�����ʹ����grpc��Ϊ��ʾ��������ʵ��һ�������ķֲ�ʽ��̨�洢ϵͳ��
 
# ����ͷ�ļ�����
 * **include/node.h** PhxPaxos����ҪAPI��������鿪���ߴ����￪ʼ��
 * **include/options.h** ����PhxPaxos�����һЩ�����Լ��ɶ��Ƶ�ѡ�
 * **include/sm.h** ״̬���Ļ��ࡣ
 * **include/def.h** ����ֵ���塣
 * **include/network.h** ����ģ��ĳ��������������ʹ���Լ�������Э�飬ͨ��������Щ����ʵ������ģ����Զ��塣
 * **include/storage.h** �洢ģ��ĳ�������
 * **include/log.h** ��־ģ��ĳ�������
 * **include/breakpoint.h** �ϵ��������һ�������ʵ���Լ��ļ�ء�
 
# ��α���
### ����ǰ�ĵ�������׼��
�������ǿ�һ�¸�Ŀ¼��������ϵ�����£�

| Ŀ¼               | �������             | �ڲ�����                           | ������������     |
| ------------------ | -------------------- | ---------------------------------- | ---------------- |
| ��Ŀ¼             | libphxpaxos.a        | ��                                 | protobuf,leveldb |
| plugin             | libphxpaxos_plugin.a | libphxpaxos.a                      | glog             |
| sample/phxelection | ��ִ�г���           | libphxpaxos.a,libphxpaxos_plugin.a | ��               |
| sample/phxecho     | ��ִ�г���           | libphxpaxos.a,libphxpaxos_plugin.a | ��               |
| sample/phxkv       | ��ִ�г���           | libphxpaxos.a,libphxpaxos_plugin.a | grpc             |
| src/ut             | ��Ԫ����             | ��                                 | gtest,gmock      |

�������ǵ�Phxpaxo(libphxpaxos.a)��⣬ֻ��Ҫprotobuf��leveldb�����������⣻����������Ŀ¼����Ҫglog��grpc�������⡣
�ڱ���ǰ����Ҫ��׼������Щ�������⣬�������ǵ�third_partyĿ¼������ֱ�ӷ��ã�Ҳ����ͨ����������ʽ��

### ���뻷��
 * Linux��
 * GCC-4.8�����ϰ汾��
 
### ���밲װ����
###### ����libphxpaxos.a
 
    ��PhxPaxos��Ŀ¼�£�ִ��autoinstall.sh
    make
    make install

###### ����libphxpaxos_plugin.a
 
    ��pluginĿ¼��
    make
    make install

# ���Ƕ��PhxPaxos���Լ��Ĵ���
### ѡ��һ����������
����ѡ��sampleĿ¼�����PhxEcho��˵��PhxPaxos��ʹ�÷�����Echo�����Ǳ�дRPC����ĳ������Ժ�����
���ǳ���ͨ��Ƕ��PhxPaxos�Ĵ��룬ʹ�����ǵ�Echo������չ����̨������

���������������е�EchoServer����ͷ�ļ��ඨ�����£�
```c++
class PhxEchoServer
{
public:
    PhxEchoServer();
    ~PhxEchoServer();

    int Echo(const std::string & sEchoReqValue, std::string & sEchoRespValue);
};
```
���������ǻ����������Ƕ��PhxPaxos�Ĵ��롣

### ʵ��һ��״̬��
�������Ƕ���һ��״̬����PhxEchoSM�������̳���StateMachine�࣬���£�
```c++
class PhxEchoSM : public phxpaxos::StateMachine
{
public:
    PhxEchoSM();

    bool Execute(const int iGroupIdx, const uint64_t llInstanceID, 
            const std::string & sPaxosValue, phxpaxos::SMCtx * poSMCtx);

    const int SMID() const { return 1; }
};
```
��Ϊһ��PhxPaxos����ͬʱ���ض��״̬����������ҪSMID()��������������״̬����Ψһ��ʶID��

����ExecuteΪ״̬��״̬ת�ƺ���������ΪsPaxosValue�� PhxPaxos��֤��̨��������ִ����ͬϵ�е�Execute(sPaxosValue)��
�Ӷ����ǿһ���ԡ�������ʵ�����£�
```c++
bool PhxEchoSM :: Execute(const int iGroupIdx, const uint64_t llInstanceID, 
        const std::string & sPaxosValue, SMCtx * poSMCtx)
{
    printf("[SM Execute] ok, smid %d instanceid %lu value %s\n", 
            SMID(), llInstanceID, sPaxosValue.c_str());

    //only commiter node have SMCtx.
    if (poSMCtx != nullptr && poSMCtx->m_pCtx != nullptr)
    {   
        PhxEchoSMCtx * poPhxEchoSMCtx = (PhxEchoSMCtx *)poSMCtx->m_pCtx;
        poPhxEchoSMCtx->iExecuteRet = 0;
        poPhxEchoSMCtx->sEchoRespValue = sPaxosValue;
    }   

    return true;
}
```
���ǽ���printһ�����sPaxosValue����Ϊ�������һ����֤��

�����Ĳ���������һ��İ��������SMCtx�����£�
```c++
class SMCtx
{
public:
    SMCtx();
    SMCtx(const int iSMID, void * pCtx);

    int m_iSMID;
    void * m_pCtx;
};
```
SMCtx���Ͳ�����Ϊһ�������ģ���д�����ṩ����ô�ṩ������ᵽ��������PhxPaxos����Execute���������մ��ݸ��û�ʹ�á�

m_iSMID�������ᵽ��SMID()�������Ӧ��PhxPaxos�Ὣ��������Ĵ���SMID()����m_iSMID��״̬����
m_pCtx���¼���û��Զ�������������ݵ����ڵ�ַ��

>Execute�����������Ĳ�����������д�����ڽ��̿��Ի�ã��������������ָ��Ϊnullptr��
����Execute�ڴ������������ʱ��ע��Ҫ���п�ָ����жϡ�

����չʾ�����ǵ�Echo�������������Ͷ��壺
```c++
class PhxEchoSMCtx
{
public:
    int iExecuteRet;
    std::string sEchoRespValue;

    PhxEchoSMCtx()
    {   
        iExecuteRet = -1; 
    }   
};
```
ͨ��iExecuteRet���Ի��Execute��ִ�������ͨ��sEchoRespValue���Ի��Execute�����sEchoReqValue��

���������ϼ����࣬���ǹ������Լ���״̬���Լ�״̬ת�ƺ�����

>��С��Ҫ�㣺����������һ�����еķ���ģ��ʹ��ั��������ô��Ҫ���Ľ������ǳ�����ķ����߼���
ʹ����һ��Execute���������˶��ѡ�

### ����PhxPaxos
�ڱ�д��Echo״̬��֮�󣬽�����Ҫ���ľ�������PhxPaxos�����ҹ�����״̬����

�������Ƕ�ԭ�е�EchoServer�����һ���޸ģ����£�
```c++
class PhxEchoServer
{
public:
    PhxEchoServer(const phxpaxos::NodeInfo & oMyNode, const phxpaxos::NodeInfoList & vecNodeList);
    ~PhxEchoServer();

    int RunPaxos();
    int Echo(const std::string & sEchoReqValue, std::string & sEchoRespValue);

    phxpaxos::NodeInfo m_oMyNode;
    phxpaxos::NodeInfoList m_vecNodeList;

    phxpaxos::Node * m_poPaxosNode;
    PhxEchoSM m_oEchoSM;
};
```
���캯�������˼���������oMyNode��ʶ������IP/PORT��Ϣ��vecNodeList��ʶ�ั����Ⱥ�����л�����Ϣ��
��Щ�������Ͷ���PhxPaxos��Ԥ�����͡�

˽�г�Ա���棬����m_oMyNode��m_vecNodeList֮�⣬m_oEchoSM�����Ǹոձ�д��״̬���࣬m_poPaxosNode�����
�˱���������Ҫ���е�PhxPaxosʵ��ָ�롣

����ͨ������RunPaxos����������PhxPaxosʵ�������������ʵ�����£�
```c++
int PhxEchoServer :: RunPaxos()
{
    Options oOptions;

    int ret = MakeLogStoragePath(oOptions.sLogStoragePath);
    if (ret != 0)
    {   
        return ret;
    }   

    //this groupcount means run paxos group count.
    //every paxos group is independent, there are no any communicate between any 2 paxos group.
    oOptions.iGroupCount = 1;

    oOptions.oMyNode = m_oMyNode;
    oOptions.vecNodeInfoList = m_vecNodeList;

    GroupSMInfo oSMInfo;
    oSMInfo.iGroupIdx = 0;
    //one paxos group can have multi state machine.
    oSMInfo.vecSMList.push_back(&m_oEchoSM);
    oOptions.vecGroupSMInfoList.push_back(oSMInfo);

    //use logger_google to print log
    LogFunc pLogFunc;
    ret = LoggerGoogle :: GetLogger("phxecho", "./log", 3, pLogFunc);
    if (ret != 0)
    {   
        printf("get logger_google fail, ret %d\n", ret);
        return ret;
    }   

    //set logger
    oOptions.pLogFunc = pLogFunc;

    ret = Node::RunNode(oOptions, m_poPaxosNode);
    if (ret != 0)
    {   
        printf("run paxos fail, ret %d\n", ret);
        return ret;
    }   

    printf("run paxos ok\n");
    return 0;
}
```
Option���ͱ����������������PhxPaxosʵ�������в����Լ�ѡ�

MakeLogStoragePath�����������Ǵ��PhxPaxos���������ݵ�Ŀ¼·�������ø�oOptions.sLogStoragePath��
����oOptions.iGroupCountΪ1����ʶ������ͬʱ���ж��ٸ�PhxPaxosʵ��������ͨ��GroupIdx����ʶʵ����
�䷶ΧΪ[0,oOptions.iGroupCount)������֧�ֲ������ж��ʵ����ÿ��ʵ��������������ͬʵ��ֱ�����κι�����
֧�ֶ�ʵ����Ŀ�Ľ�����Ϊ�������ǿ��Թ���ͬһ��IP/PORT��

Ȼ�����úñ���IP/PORT��Ϣ�Լ����л�������Ϣ��

�������ǳ���Ҫ��һ�����������Ǹղ�ʵ�ֵ�״̬����

oOptions.vecGroupSMInfoList�����˶��PhxPaxosʵ����Ӧ��״̬���б�����һ��GroupSMInfo������顣

GroupSMInfo���ͣ���������һ��PhxPaxosʵ����Ӧ��״̬���б�GroupSMInfo.iGroupIdx��ʶ���ʵ�����������ǵ�GroupCount����Ϊ1��
����GroupIdx����Ϊ0��vecSMList��ʶ��Ҫ���ص�״̬���б�����һ��״̬������ָ������顣

���ú����ǵ���־������������������pluginĿ¼��glog��־������

��󣬵���Node::RunNode(oOptions, m_poPaxosNode)���������ѡ�������гɹ�����������0��
����m_poPaxosNodeָ����������е�PhxPaxosʵ��������PhxPaxosʵ�������гɹ��ˡ�

# ��������
����չʾ������EchoServer��Echo�������Ӷ����ߴ�����ʹ��PhxPaxos������д���������£�
```c++
int PhxEchoServer :: Echo(const std::string & sEchoReqValue, std::string & sEchoRespValue)
{
    SMCtx oCtx;
    PhxEchoSMCtx oEchoSMCtx;
    //smid must same to PhxEchoSM.SMID().
    oCtx.m_iSMID = 1;
    oCtx.m_pCtx = (void *)&oEchoSMCtx;

    uint64_t llInstanceID = 0;
    int ret = m_poPaxosNode->Propose(0, sEchoReqValue, llInstanceID, &oCtx);
    if (ret != 0)
    {   
        printf("paxos propose fail, ret %d\n", ret);
        return ret;
    }   

    if (oEchoSMCtx.iExecuteRet != 0)
    {   
        printf("echo sm excute fail, excuteret %d\n", oEchoSMCtx.iExecuteRet);
        return oEchoSMCtx.iExecuteRet;
    }   

    sEchoRespValue = oEchoSMCtx.sEchoRespValue.c_str();

    return 0;
}
```
���ȶ������������ͱ���oEchoSMCtx��Ȼ���������ָ�����õ�״̬��������oCtx.m_pCtx���棬
ͬʱ��������oCtx.m_iSMIDΪ1�������Ǹոձ�д��״̬����SMID()���Ӧ����ʶ������Ҫ�������������SMIDΪ1��״̬����Execute������

Ȼ�����m_poPaxosNode->Propose����һ������GroupIdx����0����������ϣ����ʵ��0����д�����󣬵ڶ��������������ǵ��������ݡ�
llInstanceID �����ǻ�õĻزΣ����ID��һ��ȫ�ֵ�����ID�����һ�������������ǵ������ġ�

���ִ�гɹ���������0��ͨ�������ļ��ɻ��sEchoRespValue��

�������ϼ������裬���Ǿͽ�һ��������Echo�������������һ������汾��

### ����Ч��
����չʾ���Ǳ�д�Ķั��Echo���������Ч������������������˼·�Լ�ʵ��һ�飬����ֱ�ӱ������ǵ�sample/phxechoĿ¼�ĳ���

��������һ��ӵ����������phxecho��Ⱥ�����е�һ̨������
```c++
run paxos ok
echo server start, ip 127.0.0.1 port 11112

please input: <echo req value>
hello phxpaxos:)
[SM Execute] ok, smid 1 instanceid 0 value hello phxpaxos:)
echo resp value hello phxpaxos:)
```
IPΪ127.0.0.1��PORTΪ11112�����Կ������Ǹ�������һ��EchoReqValueΪ��hello phxpaxos:)����
Ȼ�����ǿ���Execute��printf�������[SM Execute] ok...���������ͨ�������Ļ��EchoRespValueΪ��ͬ�ġ�hello phxpaxos:)����

��������������������������������ǵڶ�̨������
```c++
run paxos ok
echo server start, ip 127.0.0.1 port 11113

please input: <echo req value>
[SM Execute] ok, smid 1 instanceid 0 value hello phxpaxos:)
```
IPΪ127.0.0.1��PORTΪ11113�� ���Կ�������Execute��������ϢҲ��ӡ���˳�����valueͬ��Ϊ��hello phxpaxos:)����

# ʹ��PhxPaxos��Master�������Server�ṩһ��ѡ�ٹ���
�����Ƚ���һ��Master�Ķ��壬Master��ָ�ڶ�̨���������ļ������棬��һʱ�̣�ֻ��һ̨������Ϊ�Լ���Master����û���κλ�����Ϊ�Լ���Master��

������ܷǳ�ʵ�á���������ôһ����̨������ɵļ�Ⱥ����ϣ����һʱ��ֻ��һ̨�������ṩ�������Ŵ�ҿ��ܻ����������ĳ�����
��ͨ��������������ʹ��ZooKeeper����ֲ�ʽ������ôʹ�����ǵ�Master���ܣ�ֻ���д�̶̵ļ�ʮ�д��룬
���ɸ������еķ����޷�����������������������һЩ�Ӵ��ģ�顣
����
����չʾ���Ƕ��Master���Լ��Ĵ������档

�������ǹ���һ��ѡ����PhxElection������๩���е�ģ�����ʹ�ã����£�
```c++
class PhxElection
{
public:
    PhxElection(const phxpaxos::NodeInfo & oMyNode, const phxpaxos::NodeInfoList & vecNodeList);
    ~PhxElection();

    int RunPaxos();
    const phxpaxos::NodeInfo GetMaster();
    const bool IsIMMaster();

private:
    phxpaxos::NodeInfo m_oMyNode;
    phxpaxos::NodeInfoList m_vecNodeList;
    phxpaxos::Node * m_poPaxosNode;
};
```
������ṩ�������ܺ�����GetMaster��õ�ǰ��Ⱥ��Master��IsIMMaster�ж��Լ��Ƿ�ǰMaster��

RunPaxos������PhxPaxos�ĺ������������£�

```c++
int PhxElection :: RunPaxos()
{
    Options oOptions;

    int ret = MakeLogStoragePath(oOptions.sLogStoragePath);
    if (ret != 0)
    {   
        return ret;
    }   

    oOptions.iGroupCount = 1;

    oOptions.oMyNode = m_oMyNode;
    oOptions.vecNodeInfoList = m_vecNodeList;

    //open inside master state machine
    GroupSMInfo oSMInfo;
    oSMInfo.iGroupIdx = 0;
    oSMInfo.bIsUseMaster = true;

    oOptions.vecGroupSMInfoList.push_back(oSMInfo);

    ret = Node::RunNode(oOptions, m_poPaxosNode);
    if (ret != 0)
    {   
        printf("run paxos fail, ret %d\n", ret);
        return ret;
    }   

    //you can change master lease in real-time.
    m_poPaxosNode->SetMasterLease(0, 3000);

    printf("run paxos ok\n");
    return 0;
}
```
��Echo��һ�����ǣ�������ǲ�����Ҫʵ���Լ���״̬��������ͨ����oSMInfo.bIsUseMaster����Ϊtrue�������������õ�һ��Master״̬����
��ͬ�ģ�ͨ��Node::RunNode���ɻ��PhxPaxos��ʵ��ָ�롣ͨ��SetMasterLease������ʱ�޸�Master����Լʱ�䡣
�������ͨ�����ָ���ü�Ⱥ��Master��Ϣ���������£�

```c++
const phxpaxos::NodeInfo PhxElection :: GetMaster()
{
    //only one group, so groupidx is 0.
    return m_poPaxosNode->GetMaster(0);
}

const bool PhxElection :: IsIMMaster()
{
    return m_poPaxosNode->IsIMMaster(0);
}
```
ͨ������򵥵�ѡ���࣬ÿ̨���������Ի�֪��ǰ��Master��Ϣ��

# ��ӭʹ�ã���ӭ�������ǵĽ���:)
