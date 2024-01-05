### props は展開せずに使う

理由: props から渡ってた値だと区別がつけやすいので

【GOOD】
export const MyPage: React.FC = (props) => {
return <p>{props.title}</p>
}
【BAD】
export const MyPage: React.FC = ({title}) => {
return <p>{title}</p>
}
