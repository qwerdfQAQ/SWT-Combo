# SWT-Combo
SWT Combo 组件重写模糊匹配方式

import org.eclipse.jface.fieldassist.ComboContentAdapter;
import org.eclipse.jface.fieldassist.ContentProposal;
import org.eclipse.jface.fieldassist.ContentProposalAdapter;
import org.eclipse.jface.fieldassist.IContentProposal;
import org.eclipse.jface.fieldassist.IContentProposalProvider;

/*
	 * Combo 模糊匹配 Combo组件必须初始化items
	 */
	public static void fuzzyMatchAtCombo(Combo combo) {
		String[] items = combo.getItems();
		IContentProposalProvider proposalProvider = new IContentProposalProvider() {
			@Override
			public IContentProposal[] getProposals(String contents, int position) {
				ArrayList<IContentProposal> validProposals = new ArrayList<IContentProposal>();
				for (String prop : items) {
					contents = contents.substring(0, position);
          //匹配方式，可以自行修改
					if ((prop.toUpperCase()).contains((contents.toUpperCase()))) {
						validProposals.add(new ContentProposal(prop));
					}
				}
				return validProposals.toArray(new IContentProposal[validProposals.size()]);
			}
		};

		ContentProposalAdapter adapter = new ContentProposalAdapter(combo, new ComboContentAdapter(), proposalProvider,
				null, null);
		adapter.setPropagateKeys(true);
		adapter.setProposalAcceptanceStyle(ContentProposalAdapter.PROPOSAL_REPLACE);
	}
