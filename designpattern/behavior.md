策略模式
策略类接口 + 多个具体策略类
外加策略类Factory，把各个策略类放到map里，根据一个string来选择对应的策略类。

@Component
public class DatasetInfoStrategyFactory {

    @Autowired
    Map<String, DataSetInfoParseStrategy> dataSetInfoParseStrategyMap;

    public DataSetInfoParseStrategy getParser(String parser) {
        return dataSetInfoParseStrategyMap.get(parser);
    }
}
