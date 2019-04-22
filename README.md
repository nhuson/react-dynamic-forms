
import {
    Form,
    Schema,
    Field,
    Chooser,
    TextEdit,
    formGroup,
    formList,
    FormEditStates
} from "@nhuson/react-dynamic-forms";

class EmailForm extends React.Component {
    static defaultValues = { id: 2, amount: 1 };

    static schema = (
        <Schema>
            <Field
                name="amount"
                defaultValue=""
                label="amount"
                required={true}
            />
            <Field name="id" defaultValue={1} label="Type" required={true} />
        </Schema>
    );

    emailTypes() {
        return Immutable.fromJS([{ id: 1, label: "Onion" }, { id: 2, label: "Lemon" }]);
    }

    emailTypeLabel() {
        let result;
        this.emailTypes().forEach(obj => {
            if (obj.id === this.props.value.get("id")) {
                result = obj.label;
            }
        });
        return result;
    }

    render() {
        const {
            onChange,
            onMissingCountChange,
            value = EmailForm.defaultValues
        } = this.props;
        const callbacks = { onChange, onMissingCountChange };
        if (this.props.edit) {
            return (
                <Form
                    name={this.props.name}
                    schema={EmailForm.schema}
                    value={value}
                    edit={FormEditStates.ALWAYS}
                    labelWidth={50}
                    {...callbacks}
                >
                    <Chooser
                        field="id"
                        choiceList={this.emailTypes()}
                        disableSearch={false}
                        width={150}
                        value={1}
                    />
                    <TextEdit field="amount" width={300} />
                </Form>
            );
        } else {
            return (
                <Form
                    name={this.props.name}
                    schema={EmailForm.schema}
                    value={value}
                    edit={FormEditStates.TABLE}
                    labelWidth={50}
                    {...callbacks}
                >
                    <TextEdit field="amount" width={250} />
                    <Chooser
                        field="id"
                        choiceList={this.emailTypes()}
                        disableSearch={false}
                        width={250}
                        value={1}
                    />
                </Form>
            );
        }
    }
}

const EmailList = formList(EmailForm);
const Ingredients = formGroup(EmailList);
